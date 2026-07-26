---
title: Two-Layer Guarding and Non-Bypassable Veto Architecture
aliases: [Two-Layer Guarding, Safety Veto Architecture, Defense-in-Depth Agent Safety]
type: knowledge
project: global
status: evergreen
created: 2026-07-26
updated: 2026-07-26
tags: [agentic-ai, multi-agent, safety-veto, runtime-guards, security, privacy-dpo, defense-in-depth]
source: internal-harness-repo
---

# Two-Layer Guarding and Non-Bypassable Veto Architecture

> [!abstract] TL;DR
> The **Two-Layer Guarding and Non-Bypassable Veto Architecture** provides defense-in-depth for AI-assisted software delivery. It decouples safety into two distinct layers: **Layer 1 (Review-Time Non-Bypassable Veto)** uses isolated AI personas (`safety-reviewer`, `security-engineer`, `privacy-counsel`) with absolute veto power (`merged: false`) over safety surfaces; **Layer 2 (Runtime Tool Guard Hooks)** uses deterministic, cross-platform Node hooks (`guard-tool-call.js`, `secret-scan.js`) to block secret file access, credential leakage, protected branch force-pushes, and catastrophic commands *before tool execution occurs*.

---

## 1. The Defense-in-Depth Safety Model

AI coding agents operating in autonomous or semi-autonomous modes present two distinct safety risks:
1. **Semantic / Design Safety Defects**: Shipping code that violates user safety, privacy laws (GDPR/DPO), or security standards due to agent confirmation bias or incomplete prompts.
2. **Runtime Operational Catastrophes**: Accidentally reading production secrets (`.env`), pushing credentials to LLM providers, deleting database tables, or force-pushing to protected main branches.

To eliminate both risk classes without obstructing developer productivity, the harness implements a **two-layer defense-in-depth topology**:

```mermaid
graph TD
    UserPrompt[User Prompt / Command] --> L2_Prompt{Layer 2: secret-scan.js}
    L2_Prompt -- Secret Detected --> BlockPrompt[Block Prompt Submission]
    L2_Prompt -- Clean --> AgentExec[Agent Execution]
    
    AgentExec --> ToolCall[Tool Invocation: Bash / File Edit]
    ToolCall --> L2_Tool{Layer 2: guard-tool-call.js}
    L2_Tool -- Catastrophic Cmd / Secret File --> BlockTool[Deny Tool Execution]
    L2_Tool -- Clean --> FileSys[File System / Shell Execution]
    
    FileSys --> ReviewPhase[Beat D / Gate Phase]
    ReviewPhase --> L1_Veto{Layer 1: Tripartite Review Gate}
    L1_Veto -- Safety / Security / Privacy Veto --> BlockMerge[Veto: Merged = False]
    L1_Veto -- All Co-Gates Cleared --> OpenMR[Open Dark MR for Human Merge]
```

---

## 2. Layer 1: Review-Time Non-Bypassable Veto Gate

Layer 1 operates during feature authoring and pre-ship verification (`/implement` and `/ship`). It evaluates semantic correctness, architectural integrity, legal compliance, and safety surface adherence.

### 1. The Tripartite Co-Gate Cluster
When a feature touches a declared **safety surface** (`.claude/project-profile.md §5`), three specialized personas act as independent co-gatekeepers:

```
                              ┌───────────────────────────┐
                              │  feature diff / PRD spec  │
                              └─────────────┬─────────────┘
                                            │
           ┌────────────────────────────────┼────────────────────────────────┐
           ▼                                ▼                                ▼
┌────────────────────┐            ┌────────────────────┐            ┌────────────────────┐
│  safety-reviewer   │            │ security-engineer  │            │  privacy-counsel   │
│  (Product & User   │            │ (AppSec, Exploits, │            │ (Legal, DPO, PII,  │
│   Safety Veto)     │            │  Authz, Vulns)     │            │ Retention, Minors) │
└──────────┬─────────┘            └─────────┬──────────┘            └──────────┬─────────┘
           │                                │                                │
           ▼                                ▼                                ▼
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        Workflow Deterministic Veto Evaluator                           │
│     IF any co-gate returns BLOCK / REJECT ──> merged: false (NON-BYPASSABLE VETO)      │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

* **`safety-reviewer` (Product & User Safety)**: Evaluates user protection, content moderation, structural integrity, and user-facing safety guardrails. Holds an absolute veto (`merged: false`).
* **`security-engineer` (AppSec & Vulnerabilities)**: Inspects diffs for OWASP vulnerabilities, authorization bypasses (IDOR), SQL/command injection, dependency CVEs, and credential leakage. A `security-engineer` BLOCK holds the PR like a safety veto.
* **`privacy-counsel` (Data Protection Officer / DPO Gate)**: Mandated for all `data`-lane tasks, dashboards, and PII surfaces. Validates lawful basis for processing, data minimization, retention limits, cross-border transfers, and child-data protections. 

> [!IMPORTANT]
> **The Independence Guarantee**: A `privacy-counsel` or `security-engineer` BLOCK cannot be overridden by a clean `be-reviewer` code approval or project deadline. A query can be 100% syntactically correct and exploit-free, but if it over-collects user PII, `privacy-counsel` issues a non-negotiable veto.

### 2. Deterministic Workflow Enforcement
The veto is not advisory text. The orchestration script (`workflows/paired-review.example.js`) evaluates safety sign-offs programmatically:
* If `safety_surface: yes` is declared, the workflow enforces a mandatory execution step for `safety-reviewer`.
* The workflow JSON schema returns `merged: false` if any reviewer issues a veto.
* No metric improvement, deadline pressure, or secondary reviewer `APPROVE` can override a `merged: false` state.

---

## 3. Layer 2: Runtime Programmatic Guard Hooks

Layer 2 provides immediate runtime defense-in-depth. Implemented as zero-dependency, cross-platform Node.js scripts in `hooks/`, these hooks run directly on the host prior to tool execution or LLM prompt submission.

### 1. `guard-tool-call.js` (PreToolUse Safety Guard)
Intercepts tool execution requests (`Read`, `Edit`, `Write`, `MultiEdit`, `Bash`) and denies dangerous actions before they execute.

```javascript
// Example check logic in hooks/guard-tool-call.js
if (/^(Read|Edit|Write|MultiEdit)$/.test(tool)) {
  if (isSecretPath(filePath)) {
    deny('Access to a secret/credential file (.env, *.pem, *.key)');
  }
}
```

* **Secret File Protection**:
  * Blocks reading or editing `.env`, `.env.production`, `*.pem`, `*.key`, `id_rsa`, `id_ed25519`, `credentials.json`, or `secrets.yml`.
  * *Non-Secret Write Exception*: Allows writing `.env` files *only* if static analysis confirms every assigned value is non-secret (e.g., local SQLite paths like `DATABASE_URL="file:./dev.db"`). Reading existing `.env` files remains strictly blocked.
* **Catastrophic Shell Command Blocking**:
  * **Protected Branch Guard**: Blocks `git push --force` or `git push --delete` targeting `main`, `master`, `production`, or `release`. Blocks `git branch -D main`.
  * **Recursive Delete Guard**: Blocks `rm -rf /`, `rm -rf ~`, `rm -rf $HOME`, or `rm -rf /*`.
  * **Disk Wipe & Destruction Guard**: Blocks fork bombs (`:(){ :|:& };:`), drive formatting (`mkfs`), and raw block writes (`dd of=/dev/sdX`).
  * **Database Dropping Guard**: Blocks `DROP DATABASE` queries to prevent accidental schema destruction.

### 2. `secret-scan.js` (Runtime Secret Content Scanner)
While `guard-tool-call.js` blocks secret *file paths*, `secret-scan.js` inspects tool *contents* and prompt text to prevent active API keys and tokens from reaching model providers.

* **Monitored Events**: Intercepts `UserPromptSubmit` (user input prompts) and `PreToolUse` (`Bash`, `Write`, `Edit`, `MultiEdit`).
* **High-Confidence Regex Detectors**:
  * Private key blocks (`-----BEGIN PRIVATE KEY-----`).
  * AWS Access Key IDs (`AKIA[0-9A-Z]{16}`).
  * GitHub Tokens (`ghp_`, `github_pat_`).
  * GitLab Access Tokens (`glpat-`).
  * Slack, Google (`AIza`), Stripe (`sk_live_`), Anthropic (`sk-ant-`), and OpenAI (`sk-proj-`) API keys.
  * Bearer Tokens and JWTs (`eyJ...`).
  * Hardcoded credential string assignments (`api_key = "..."`).
* **The Redaction Guarantee**:
  ```
  Possible secret detected in this tool call — looks like an AWS access key id. 
  Blocked by the secret-content scanner so it does not reach the model/provider. 
  (The value is not shown.)
  ```
  Matched secret strings are **never** echoed in error logs, stdout, or stderr.

### 3. `completion-gate.js` (Autonomous Run Lock)
Attached to the `Stop` hook event, this script prevents subagents from prematurely stopping during autonomous multi-step `/deliver` runs:
* Reads `.harness/run-state.json`.
* If `state: "running"` and owned by the current active session, the hook blocks premature exit, forcing the subagent to complete its pipeline or reach an explicit human checkpoint.
* Includes a no-progress circuit breaker to prevent infinite execution loops if an agent gets stuck.

---

## 4. Layer 1 vs. Layer 2 Comparison Matrix

| Dimension | Layer 1: Review-Time Veto Gate | Layer 2: Runtime Tool Guard Hooks |
| :--- | :--- | :--- |
| **Execution Phase** | Beat D / Gate Phase (`/implement`, `/ship`) | Pre-execution (`PreToolUse`, `UserPromptSubmit`) |
| **Inspection Subject**| Semantic Code Diffs, PRD Specs, Rules | Raw Tool Calls, Shell Strings, File Paths, Prompts |
| **Evaluator** | Isolated AI Personas (`safety-reviewer`, etc.) | Deterministic Node.js Scripts (`hooks/*.js`) |
| **Scope** | Product Safety, Privacy/DPO, AppSec, WCAG AA | Secret File Access, Destructive Commands, Credential Leaks |
| **Veto Mechanism** | Workflow returns `merged: false` | Hook outputs `permissionDecision: "deny"` |
| **Override Policy** | Inviolable (Human must fix code/spec) | Env variable (`HARNESS_GUARD_DISABLE=1`) |
| **Failure Mode** | Fail-closed (Unsigned safety = Block) | Fail-safe (Hook error → Allow, avoid dev lockout) |

---

## 5. Environmental Controls & Safety Floor Guarantees

### Environment Variables & Profiling
Runtime hooks can be configured or bypassed for specific CI/local workflows via environment flags:

| Environment Variable | Target Hook | Behavior |
| :--- | :--- | :--- |
| `HARNESS_HOOK_PROFILE=minimal` | All Hooks | Disables all hooks *except* output compression (`compress-bash-output.js`). |
| `HARNESS_GUARD_DISABLE=1` | `guard-tool-call.js` | Disables file path and catastrophic shell command blocking. |
| `HARNESS_SECRET_SCAN_DISABLE=1` | `secret-scan.js` | Disables secret content scanning. |
| `HARNESS_SECRET_SCAN_WARN=1` | `secret-scan.js` | Downgrades secret scanner from hard block to non-blocking warning notice. |
| `HARNESS_COMPLETION_GATE_DISABLE=1` | `completion-gate.js` | Disables autonomous run completion locking. |

### The Inviolable Safety Floor Rule
While project-specific rules in `.claude/RULES.md` allow customizing code conventions, the harness enforces a **Deterministic Precedence Guarantee**:
* A project rule override in `RULES.md` may make a safety, privacy, or security rule **stricter**.
* An override **can never weaken or disable** the Layer 1 §5 safety veto, AppSec checks, or DPO privacy gates.

---

## 6. Code Implementations

### PreToolUse Safety Guard (`hooks/guard-tool-call.js`)

```javascript
#!/usr/bin/env node
'use strict'
// multi-agent-harness — PreToolUse safety guard (RUNTIME defense-in-depth).
//
// The harness's safety is otherwise entirely REVIEW-time (the safety-reviewer veto runs in
// /implement). A developer running ad-hoc tool calls OUTSIDE that workflow had no second line of
// defense. This hook adds a thin RUNTIME layer: it blocks a small, unambiguous set of dangerous
// tool calls BEFORE they run.
//
// It is SOFT policy, not OS containment — it does not sandbox. It's deliberately conservative
// (block only the clearly-catastrophic) and FAIL-SAFE (any error → allow, never break a dev's flow).

const prof = (process.env.HARNESS_HOOK_PROFILE || 'standard').toLowerCase()
if (prof === 'minimal' || process.env.HARNESS_GUARD_DISABLE === '1') process.exit(0)

let input = ''
process.stdin.setEncoding('utf8')
process.stdin.on('data', (d) => { input += d })
process.stdin.on('end', () => {
  try {
    const p = JSON.parse(input || '{}')
    const tool = p.tool_name || ''
    const ti = p.tool_input || {}

    const deny = (reason) => {
      process.stdout.write(JSON.stringify({
        hookSpecificOutput: {
          hookEventName: 'PreToolUse',
          permissionDecision: 'deny',
          permissionDecisionReason:
            reason + ' — blocked by the multi-agent-harness safety guard. A human should run this if intended ' +
            '(or set HARNESS_GUARD_DISABLE=1 for this session).',
        },
      }))
      process.exit(0)
    }

    // --- file tools: block reading/editing secret files ---
    if (/^(Read|Edit|Write|MultiEdit|NotebookEdit)$/.test(tool)) {
      const fp = String(ti.file_path || ti.path || ti.notebook_path || '')
      if (fp && isSecretPath(fp)) {
        const base = (fp.split(/[\\/]/).pop() || '')
        const isEnv = /^\.env(\.|$)/.test(base) && !/\.(example|sample|template|dist)$/i.test(base)
        const isWrite = /^(Edit|Write|MultiEdit|NotebookEdit)$/.test(tool)
        if (isEnv && isWrite) {
          const content = writeContent(ti)
          if (content !== null && envValuesAreNonSecret(content)) process.exit(0) // non-secret .env write → allow
        }
        deny('Access to a secret/credential file (' + fp + '); the harness never reads or edits secrets')
      }
      process.exit(0)
    }

    // --- Bash: block catastrophic / protected commands ---
    if (tool === 'Bash') {
      const c = String(ti.command || '')

      // read/copy a real .env
      if (/(?:^|[;&|]\s*)(?:sudo\s+)?(?:cat|less|more|head|tail|nano|vim?|emacs|strings|xxd|od|bat)\s+[^;&|]*\.env\b(?!\.(?:example|sample|template|dist))/i.test(c) ||
          /\b(?:cp|scp|rsync)\b[^;&|]*\.env\b(?!\.(?:example|sample|template))/i.test(c))
        deny('Reading/copying a .env secret file')

      // force-push or delete a PROTECTED branch
      if (/\bgit\s+push\b/.test(c) && /(--force\b|--force-with-lease\b|(?:^|\s)-f\b|--delete\b|(?::|\s)\+?(?:main|master)\b)/.test(c) && /\b(main|master|production|prod|release)\b/.test(c))
        deny('Force-push / delete to a protected branch (main/master); humans hold the protected-branch write path')
      if (/\bgit\s+branch\s+-D\b/.test(c) && /\b(main|master)\b/.test(c))
        deny('Deleting a protected branch')

      // catastrophic recursive delete of / ~ $HOME /*
      if (/\brm\s+(?:-[a-zA-Z]+\s+)*-?[a-zA-Z]*[rf][a-zA-Z]*[rf][a-zA-Z]*\s+(?:-[a-zA-Z]+\s+)*(\/|~|\$HOME|\$\{HOME\}|\/\*)(\s|$|\/\*?)/.test(c))
        deny('Catastrophic recursive delete of / ~ or $HOME')

      // fork bomb / disk wipe
      if (/:\(\)\s*\{\s*:\s*\|\s*:\s*&\s*\}\s*;\s*:/.test(c) || /\bmkfs(\.\w+)?\b/.test(c) ||
          /\bdd\b[^;&|]*\bof=\/dev\/(sd|nvme|disk|hd)/.test(c) || />\s*\/dev\/(sd|nvme|disk|hd)\w*/.test(c))
        deny('Disk-destroying command')

      // drop a database
      if (/\bdrop\s+database\b/i.test(c))
        deny('DROP DATABASE; humans hold prod data writes')

      process.exit(0)
    }

    process.exit(0)
  } catch (_e) {
    process.exit(0) // fail-safe: never block on a guard bug
  }
})

function writeContent (ti) {
  if (typeof ti.content === 'string') return ti.content
  if (Array.isArray(ti.edits)) return ti.edits.map((e) => String((e && e.new_string) || '')).join('\n')
  if (typeof ti.new_string === 'string') return ti.new_string
  return null
}

function envValuesAreNonSecret (content) {
  const lines = String(content).split(/\r?\n/)
  const SECRET_KEY = /(secret|token|password|passwd|pwd|api[_-]?key|access[_-]?key|private[_-]?key|client[_-]?secret|auth[_-]?token|credential|session|cookie|salt|signing|encryption|jwt|bearer|dsn|webhook)/i
  const SECRET_VAL = /^(sk-|rk_|pk_live|ghp_|gh[opsru]_|xox[baprs]-|AKIA[0-9A-Z]{12,}|ASIA[0-9A-Z]{12,}|-----BEGIN|eyJ[A-Za-z0-9_-]{10,}\.)/
  for (let raw of lines) {
    const line = raw.trim()
    if (!line || line.startsWith('#')) continue
    const m = line.replace(/^export\s+/, '').match(/^([A-Za-z_][A-Za-z0-9_]*)\s*=\s*(.*)$/)
    if (!m) return false
    const key = m[1]
    let val = m[2].trim().replace(/^["']|["']$/g, '')
    if (!/^["']/.test(m[2].trim())) val = val.replace(/\s+#.*$/, '').trim()
    if (SECRET_KEY.test(key)) return false
    if (SECRET_VAL.test(val)) return false
    if (/:\/\/[^/@\s]+:[^/@\s]+@/.test(val)) return false
    if (val.length >= 20 && /[A-Za-z]/.test(val) && /[0-9]/.test(val) && /^[A-Za-z0-9+/_=.\-]+$/.test(val) && !/^(file|https?|postgres|postgresql|mysql|mongodb|redis|sqlite):/i.test(val)) return false
  }
  return true
}

function isSecretPath(fp) {
  const base = (fp.split(/[\\/]/).pop() || '')
  if (/^\.env(\.|$)/.test(base) && !/\.(example|sample|template|dist)$/i.test(base)) return true
  if (/\.(pem|p12|pfx)$/i.test(base)) return true
  if (/\.key$/i.test(base) && !/\.pub\.key$/i.test(base)) return true
  if (/^(secrets?|credentials?)\.(json|ya?ml|txt|env)$/i.test(base)) return true
  return false
}
```

### Secret Content Scanner (`hooks/secret-scan.js`)

```javascript
#!/usr/bin/env node
'use strict'
// multi-agent-harness — secret-CONTENT scanner (RUNTIME defense-in-depth). CROSS-PLATFORM Node.
//
// Scans UserPromptSubmit and tool calls for AWS/GitHub/GitLab/Slack/Stripe/OpenAI/Anthropic keys.
// Redaction Guarantee: The matched secret string is NEVER echoed in reason outputs.

var prof = (process.env.HARNESS_HOOK_PROFILE || 'standard').toLowerCase()
if (prof === 'minimal' || process.env.HARNESS_SECRET_SCAN_DISABLE === '1') process.exit(0)

var WARN_ONLY = process.env.HARNESS_SECRET_SCAN_WARN === '1'

var RULES = [
  { label: 'a private key block', re: /-----BEGIN (?:RSA |EC |OPENSSH |DSA |PGP )?PRIVATE KEY-----/ },
  { label: 'an AWS access key id', re: /\bAKIA[0-9A-Z]{16}\b/ },
  { label: 'a GitHub token', re: /\bgh[posru]_[A-Za-z0-9]{36,}\b/ },
  { label: 'a GitHub fine-grained PAT', re: /\bgithub_pat_[A-Za-z0-9_]{22,}\b/ },
  { label: 'a GitLab personal access token', re: /\bglpat-[A-Za-z0-9_-]{20,}\b/ },
  { label: 'a Slack token', re: /\bxox[baprs]-[A-Za-z0-9-]{10,}\b/ },
  { label: 'a Google API key', re: /\bAIza[0-9A-Za-z_-]{35}\b/ },
  { label: 'a Stripe live secret key', re: /\b(?:sk|rk)_live_[0-9A-Za-z]{20,}\b/ },
  { label: 'an Anthropic API key', re: /\bsk-ant-[A-Za-z0-9_-]{20,}\b/ },
  { label: 'an OpenAI API key', re: /\bsk-(?:proj-)?[A-Za-z0-9_-]{32,}\b/ },
  { label: 'a JWT', re: /\beyJ[A-Za-z0-9_-]{8,}\.[A-Za-z0-9_-]{8,}\.[A-Za-z0-9_-]{8,}\b/ },
  { label: 'a bearer token', re: /\bBearer\s+(?!<|\$|YOUR_|xxx|token\b|TOKEN\b|\.\.\.)[A-Za-z0-9._-]{20,}\b/ },
]

var ASSIGN_RE = /\b(?:api[_-]?key|secret(?:[_-]?key)?|password|passwd|access[_-]?token|auth[_-]?token|client[_-]?secret)\s*[:=]\s*['"]([^'"]{16,})['"]/i
var PLACEHOLDER_RE = /^(?:your|xxx+|todo|changeme|change-me|placeholder|example|redacted|null|none|test|dummy|fake|\.\.\.|\$\{|process\.env|req\.|<)/i

function findSecret(text) {
  if (!text || typeof text !== 'string') return null
  for (var i = 0; i < RULES.length; i++) {
    try { if (RULES[i].re.test(text)) return RULES[i].label } catch (_e) {}
  }
  try {
    var m = ASSIGN_RE.exec(text)
    if (m && m[1] && !PLACEHOLDER_RE.test(m[1].trim()) && !/^[A-Z0-9_]+$/.test(m[1].trim())) {
      return 'a hardcoded credential in an assignment'
    }
  } catch (_e) {}
  return null
}
```

### Autonomous Run Completion Gate (`hooks/completion-gate.js`)

```javascript
#!/usr/bin/env node
'use strict';
// multi-agent-harness — Stop hook. Blocks premature stops during autonomous execution loops.

const fs = require('fs');
const path = require('path');

const STATE_FILE = path.join(process.cwd(), '.harness', 'run-state.json');
const SIDECAR_FILE = path.join(process.cwd(), '.harness', 'completion-gate.sidecar.json');

function decide({ state, payload, now, sidecar, windowMs = 30 * 60 * 1000, cap = 2 }) {
  const keep = (extra) => ({ block: false, sidecar, ...extra });
  if (!state || typeof state !== 'object') return keep();
  if (payload && payload.stop_hook_active) return keep();
  if (state.state !== 'running') return keep();
  if (state.session && payload && payload.session_id && state.session !== payload.session_id) return keep();
  if (now - (Number(state.updated) || 0) > windowMs) return keep();

  const prog = Number(state.progress) || 0;
  const seen = Number.isFinite(Number(sidecar.lastProgress)) ? Number(sidecar.lastProgress) : undefined;
  const reason =
    `An autonomous ${state.command || 'run'}${state.ticket ? ' (' + state.ticket + ')' : ''} is still running ` +
    `(phase: ${state.phase || '?'}). Continue the pipeline to its next checkpoint, or mark it ` +
    `state:"paused"/"done" in .harness/run-state.json if it is genuinely finished.`;

  if (sidecar.givenUp && prog <= (seen === undefined ? -1 : seen)) return keep();

  if (seen === undefined || prog > seen) {
    return { block: true, reason, sidecar: { lastProgress: prog, noProgressBlocks: 0, givenUp: false } };
  }
  const n = (Number(sidecar.noProgressBlocks) || 0) + 1;
  if (n >= cap) {
    return { block: false, note: 'wedged (no progress) — giving up to avoid trapping session',
      sidecar: { lastProgress: prog, noProgressBlocks: n, givenUp: true } };
  }
  return { block: true, reason, sidecar: { lastProgress: prog, noProgressBlocks: n, givenUp: false } };
}
```

---

## Related Notes

- [Multi-Agent Build Harness Architecture](Multi-Agent%20Build%20Harness%20Architecture.md)
- [Multi-Agent Harness Organizational Model](Multi-Agent%20Harness%20Organizational%20Model.md)
- [Multi-Agent Harness Command Lifecycle](Multi-Agent%20Harness%20Command%20Lifecycle.md)
- [Auto-Tiering and Token Cost Discipline Architecture](Auto-Tiering%20and%20Token%20Cost%20Discipline%20Architecture.md)
- [Project Tailoring and Deterministic Rules Architecture](Project%20Tailoring%20and%20Deterministic%20Rules%20Architecture.md)


