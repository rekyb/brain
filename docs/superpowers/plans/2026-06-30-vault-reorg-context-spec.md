# Vault Reorg for Context- & Spec-Driven Agentic AI Dev — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Reorganize the `brain` Obsidian vault into a reusable knowledge base + per-project context/spec areas, with a root `CLAUDE.md` so AI agents pointed at the vault can navigate it.

**Architecture:** Pure file/folder reorganization of a Markdown vault. Notes move via `git mv` (history preserved; Obsidian resolves `[[wikilinks]]` by name so no link rewriting needed). New top-level dirs (`10-knowledge`, `20-projects`, `40-references`, `90-templates`), four templates, a root `CLAUDE.md`, frontmatter backfill (`type`/`project`), and an updated `README.md`.

**Tech Stack:** Markdown, YAML frontmatter, Obsidian conventions, git.

## Global Constraints

- Use `git mv` for every relocation — never copy+delete (history must survive).
- Do NOT rewrite `[[wikilinks]]` — Obsidian resolves by note name, not path; paths changing is fine.
- Tracking/status must NEVER be written into vault notes — ClickUp stays the source of truth for task status.
- Frontmatter schema (every note): `type: knowledge|context|spec|reference|moc|daily`, `project: <slug>` (omit/"global" for knowledge & reference), `status: draft|active|evergreen|shipped|archived`, plus existing `title/aliases/tags/created/source`. Add `updated:` when a note is edited.
- Preserve existing frontmatter fields when adding `type`/`project`.
- Source design doc: `docs/superpowers/specs/2026-06-30-vault-reorg-context-spec-design.md`.
- The 25 existing notes are under `10-notes/{design-systems,atomic-design,edbot-v2}/`.

---

### Task 1: Create the new top-level skeleton

**Files:**
- Create dirs: `10-knowledge/design-systems/`, `10-knowledge/atomic-design/`, `10-knowledge/agentic-ai/`, `20-projects/edbot-v2/context/`, `20-projects/edbot-v2/specs/`, `40-references/`, `90-templates/`, `00-inbox/`

**Interfaces:**
- Produces: the directory tree that Tasks 2–4 move files into.

- [ ] **Step 1: Create the directories**

```bash
mkdir -p 00-inbox \
  10-knowledge/design-systems 10-knowledge/atomic-design 10-knowledge/agentic-ai \
  20-projects/edbot-v2/context 20-projects/edbot-v2/specs \
  40-references 90-templates
```

- [ ] **Step 2: Add `.gitkeep` to dirs that will stay empty at first**

Git does not track empty dirs. Keep `00-inbox`, `40-references`, and `20-projects/edbot-v2/specs` present until they hold notes.

```bash
touch 00-inbox/.gitkeep 40-references/.gitkeep 20-projects/edbot-v2/specs/.gitkeep
```

- [ ] **Step 3: Verify the tree exists**

Run: `find 00-inbox 10-knowledge 20-projects 40-references 90-templates -type d | sort`
Expected: all 8 directories listed, no error.

- [ ] **Step 4: Commit**

```bash
git add 00-inbox 40-references 20-projects/edbot-v2/specs
git commit -m "chore: scaffold new vault top-level structure"
```

---

### Task 2: Move the knowledge notes (design-systems + atomic-design)

**Files:**
- Move: `10-notes/design-systems/*` → `10-knowledge/design-systems/`
- Move: `10-notes/atomic-design/Atomic Design with AI Agents and MCP.md` → `10-knowledge/agentic-ai/`
- Move: remaining `10-notes/atomic-design/*` → `10-knowledge/atomic-design/`

**Interfaces:**
- Consumes: directories from Task 1.
- Produces: populated `10-knowledge/` tree.

- [ ] **Step 1: Move design-systems (all 11 notes)**

```bash
git mv 10-notes/design-systems/* 10-knowledge/design-systems/
```

- [ ] **Step 2: Move the AI/MCP note into agentic-ai**

```bash
git mv "10-notes/atomic-design/Atomic Design with AI Agents and MCP.md" 10-knowledge/agentic-ai/
```

- [ ] **Step 3: Move the remaining atomic-design notes**

```bash
git mv 10-notes/atomic-design/* 10-knowledge/atomic-design/
```

- [ ] **Step 4: Verify counts (11 design-systems, 8 atomic-design, 1 agentic-ai)**

Run: `for d in design-systems atomic-design agentic-ai; do printf "%s: " "$d"; ls "10-knowledge/$d" | wc -l; done`
Expected: `design-systems: 11`, `atomic-design: 8`, `agentic-ai: 1`

- [ ] **Step 5: Commit**

```bash
git add -A
git commit -m "refactor: move design-systems & atomic-design into 10-knowledge"
```

---

### Task 3: Move the Edbot project notes (MOC + context)

**Files:**
- Move: `10-notes/edbot-v2/Edbot v2.md` → `20-projects/edbot-v2/edbot-v2.md`
- Move: 4 remaining Edbot notes → `20-projects/edbot-v2/context/`
- Remove: empty `10-notes/`

**Interfaces:**
- Consumes: directories from Task 1.
- Produces: populated `20-projects/edbot-v2/` with MOC at root and 4 context notes.

- [ ] **Step 1: Move the MOC (hub note) to the project root**

```bash
git mv "10-notes/edbot-v2/Edbot v2.md" 20-projects/edbot-v2/edbot-v2.md
```

- [ ] **Step 2: Move the four spoke notes into context/**

```bash
git mv "10-notes/edbot-v2/Edbot Workspace (Meta-Repo).md" \
       "10-notes/edbot-v2/Module Contract (Core-Module-Integration).md" \
       "10-notes/edbot-v2/Edbot Roadmap & Epics.md" \
       "10-notes/edbot-v2/LEDGER Admin Prototype.md" \
       20-projects/edbot-v2/context/
```

- [ ] **Step 3: Remove the now-empty old tree**

```bash
rmdir 10-notes/edbot-v2 10-notes/atomic-design 10-notes/design-systems 10-notes 2>/dev/null; true
```

- [ ] **Step 4: Verify the project layout and that 10-notes is gone**

Run: `ls 20-projects/edbot-v2 && echo "--- context ---" && ls 20-projects/edbot-v2/context && echo "--- old ---" && ls 10-notes 2>&1`
Expected: `edbot-v2.md`, `context`, `specs` at project root; 4 notes in `context/`; `ls: cannot access '10-notes'` for old.

- [ ] **Step 5: Commit**

```bash
git add -A
git commit -m "refactor: move edbot-v2 notes into 20-projects (MOC + context)"
```

---

### Task 4: Add the four templates

**Files:**
- Create: `90-templates/knowledge-note.md`, `90-templates/context-note.md`, `90-templates/spec.md`, `90-templates/project-index.md`

**Interfaces:**
- Produces: reusable templates referenced by `CLAUDE.md` (Task 6).

- [ ] **Step 1: Create `90-templates/knowledge-note.md`**

```markdown
---
title: 
aliases: []
type: knowledge
project: global
status: evergreen
created: 2026-06-30
updated: 2026-06-30
tags: []
source: 
---

# 

> [!abstract] TL;DR
> One-paragraph summary.

## 

## Related

- [[]]

## Sources

- 
```

- [ ] **Step 2: Create `90-templates/context-note.md`**

```markdown
---
title: 
aliases: []
type: context
project: 
status: evergreen
created: 2026-06-30
updated: 2026-06-30
tags: []
source: 
---

# 

> [!abstract] TL;DR
> What this is and why it matters — durable understanding, not a task.

## How it works

## Why it is this way

## Related

- [[]]

## Sources

- 
```

- [ ] **Step 3: Create `90-templates/spec.md`**

```markdown
---
title: 
aliases: []
type: spec
project: 
status: draft
created: 2026-06-30
updated: 2026-06-30
tags: []
source: 
---

# 

> [!abstract] Goal
> One sentence: what this builds and the outcome.

## Background

Links to the durable context this relies on: [[]]

## Requirements

- 

## Non-goals

- 

## Acceptance criteria

- [ ] 

## Affected repos / files

- 

## Open questions

- 

> [!note] Tracking
> Status/progress is tracked in ClickUp, NOT in this note.
```

- [ ] **Step 4: Create `90-templates/project-index.md`**

```markdown
---
title: 
aliases: []
type: moc
project: 
status: active
created: 2026-06-30
updated: 2026-06-30
tags: [moc]
source: 
---

# 

> [!abstract] TL;DR
> What the project is, in 2–3 sentences.

## The cluster

| Note | Covers |
| ---- | ------ |
| [[]] | |

## Context

Durable understanding lives in `context/`. Build intent lives in `specs/`.

## Related

- [[]]

## Sources

- 
```

- [ ] **Step 5: Verify all four templates exist**

Run: `ls 90-templates/`
Expected: `context-note.md  knowledge-note.md  project-index.md  spec.md`

- [ ] **Step 6: Commit**

```bash
git add 90-templates/
git commit -m "feat: add knowledge/context/spec/project-index templates"
```

---

### Task 5: Backfill `type` and `project` frontmatter on existing notes

**Files:**
- Modify: all notes under `10-knowledge/` (add `type: knowledge`, `project: global`)
- Modify: all notes under `20-projects/edbot-v2/context/` (add `type: context`, `project: edbot-v2`)
- Modify: `20-projects/edbot-v2/edbot-v2.md` (add `type: moc`, `project: edbot-v2`)

**Interfaces:**
- Consumes: relocated notes from Tasks 2–3.
- Produces: notes filterable by `type`/`project`, matching the schema `CLAUDE.md` documents.

> Notes already have `title/aliases/tags/created/status/source`. Insert the two new keys immediately after the `title:` line of each file's frontmatter. Do this per-file with an editor rather than a blind sed, so existing fields are preserved. Leave `status` values as-is.

- [ ] **Step 1: Add `type: knowledge` + `project: global` to every `10-knowledge` note**

For each file in `10-knowledge/design-systems/*.md`, `10-knowledge/atomic-design/*.md`, `10-knowledge/agentic-ai/*.md`, insert after the `title:` line:

```yaml
type: knowledge
project: global
```

- [ ] **Step 2: Add `type: context` + `project: edbot-v2` to every context note**

For each file in `20-projects/edbot-v2/context/*.md`, insert after the `title:` line:

```yaml
type: context
project: edbot-v2
```

- [ ] **Step 3: Add `type: moc` + `project: edbot-v2` to the MOC**

In `20-projects/edbot-v2/edbot-v2.md`, insert after the `title:` line:

```yaml
type: moc
project: edbot-v2
```

- [ ] **Step 4: Verify every note now has a `type:` line**

Run: `for f in $(find 10-knowledge 20-projects -name '*.md'); do grep -q '^type:' "$f" || echo "MISSING type: $f"; done; echo "checked $(find 10-knowledge 20-projects -name '*.md' | wc -l) notes"`
Expected: no `MISSING` lines; `checked 25 notes`.

- [ ] **Step 5: Verify project tagging is correct**

Run: `grep -L 'project: edbot-v2' 20-projects/edbot-v2/context/*.md 20-projects/edbot-v2/edbot-v2.md`
Expected: no output (every project note tagged `edbot-v2`).

- [ ] **Step 6: Commit**

```bash
git add -A
git commit -m "chore: backfill type/project frontmatter on existing notes"
```

---

### Task 6: Write the root `CLAUDE.md`

**Files:**
- Create: `CLAUDE.md`

**Interfaces:**
- Consumes: the finished structure (Tasks 1–5) and templates (Task 4) it describes.
- Produces: the agent entry point for the vault.

- [ ] **Step 1: Create `CLAUDE.md` with the full ruleset**

```markdown
# CLAUDE.md — How to use this vault

This is **`brain`**, an Obsidian knowledge vault. It holds **context** (durable understanding) and **specs** (what to build). It is **not** a code repository — there is no app to run here. Every note is plain Markdown with YAML frontmatter, linked with `[[wikilinks]]`.

## What you're here for

Agents read this vault as **background context** while working in separate code repos (e.g. the Edbot app repos). Use it to orient: architecture, conventions, design knowledge, and per-project understanding.

## Folder map

| Path | Holds |
| ---- | ----- |
| `00-inbox/` | Quick capture, unprocessed. Triage out of here. |
| `10-knowledge/` | Reusable, **project-agnostic** evergreen notes. `design-systems/`, `atomic-design/`, `agentic-ai/`. |
| `20-projects/<project>/` | Per-project area. `<project>.md` is the MOC/index. |
| `20-projects/<project>/context/` | Durable **understanding** of the project — how it works and why. |
| `20-projects/<project>/specs/` | Authoritative **specs** — what to build, with acceptance criteria. |
| `40-references/` | External sources, clippings, quotes. |
| `90-templates/` | Note templates: `knowledge-note`, `context-note`, `spec`, `project-index`. |
| `attachments/` | Binaries (images, PDFs). |

## Where to start (reading)

1. For a project, open `20-projects/<project>/<project>.md` (the MOC) — it maps the cluster.
2. Read that project's `context/` to understand the system.
3. Read `specs/` for current build intent.
4. Pull cross-project conventions from `10-knowledge/`.

Filter by frontmatter when scanning: `type`, `project`, `status`.

## Frontmatter schema

\`\`\`yaml
type: knowledge | context | spec | reference | moc | daily
project: <slug>        # omit or "global" for knowledge & reference
status: draft | active | evergreen | shipped | archived
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [...]
\`\`\`

## The separation rule (important)

- **context/** = how things *are* and *why*. Durable understanding. Evergreen-leaning.
- **specs/** = what to *build* — requirements, acceptance criteria, affected repos/files. Each spec links to the `context/` it relies on.
- **Status/tracking does NOT live in this vault.** Task status is tracked in **ClickUp**. Writing live trackers into markdown previously caused cross-machine merge conflicts — do not reintroduce them.

## Writing rules (when you add notes)

- New durable understanding → `20-projects/<project>/context/` using `90-templates/context-note.md`.
- New build intent → `20-projects/<project>/specs/` using `90-templates/spec.md`.
- Reusable, project-agnostic knowledge → `10-knowledge/<area>/` using `90-templates/knowledge-note.md`.
- Rough capture → `00-inbox/`; refine and file it later.
- Always set `type` and (for project notes) `project`; bump `updated:` when you edit.
- Link related notes with `[[wikilinks]]`. One idea per note. Keep notes focused.
- Never put task status/progress in a note — that belongs in ClickUp.
```

- [ ] **Step 2: Verify the file renders and references real paths**

Run: `grep -E '10-knowledge|20-projects|90-templates|ClickUp' CLAUDE.md | head`
Expected: lines present referencing the real folders and the ClickUp rule.

- [ ] **Step 3: Commit**

```bash
git add CLAUDE.md
git commit -m "docs: add root CLAUDE.md with vault navigation rules"
```

---

### Task 7: Update `README.md` for the new layout

**Files:**
- Modify: `README.md` (the "Suggested structure" section and conventions)

**Interfaces:**
- Consumes: final structure from Tasks 1–6.
- Produces: human-facing docs that match reality and point to `CLAUDE.md`.

- [ ] **Step 1: Replace the "Suggested structure" code block**

Replace the aspirational PARA tree in `README.md` with the actual tree:

```
brain/
├── CLAUDE.md         # how AI agents read & use this vault
├── 00-inbox/         # quick capture, unprocessed
├── 10-knowledge/     # reusable, project-agnostic evergreen notes
│   ├── design-systems/
│   ├── atomic-design/
│   └── agentic-ai/
├── 20-projects/      # per-project context + specs
│   └── edbot-v2/
│       ├── edbot-v2.md   # project index (MOC)
│       ├── context/      # durable understanding
│       └── specs/        # what to build
├── 40-references/    # external sources, clippings
├── 90-templates/     # note / context / spec / project templates
└── attachments/      # images, PDFs, binaries
```

- [ ] **Step 2: Add a short "For AI agents" line under that section**

Add after the tree:

```markdown
> **For AI agents:** see [`CLAUDE.md`](CLAUDE.md) — it explains how to navigate the vault, the frontmatter schema, and the context-vs-spec rule (status lives in ClickUp, not here).
```

- [ ] **Step 3: Verify README references the new dirs and CLAUDE.md**

Run: `grep -E '10-knowledge|20-projects|CLAUDE.md' README.md`
Expected: the new folders and the `CLAUDE.md` pointer appear.

- [ ] **Step 4: Commit**

```bash
git add README.md
git commit -m "docs: update README for context/spec vault layout"
```

---

### Task 8: Final verification pass

**Files:**
- None (verification only).

- [ ] **Step 1: Confirm all 25 notes survived the move with history**

Run: `find 10-knowledge 20-projects -name '*.md' | wc -l`
Expected: `25`.

- [ ] **Step 2: Confirm git tracked the moves as renames (history preserved)**

Run: `git log --diff-filter=R --summary -1 --format='%s' | head`
Expected: a recent commit showing `rename` entries (not delete+add).

- [ ] **Step 3: Confirm no tracker/status files leaked into the vault notes**

Run: `grep -rIl -E '^status: (in.progress|in_progress|todo|doing)' 20-projects 10-knowledge || echo "clean"`
Expected: `clean`.

- [ ] **Step 4: Confirm old `10-notes/` is fully gone**

Run: `test ! -e 10-notes && echo "removed" || echo "STILL PRESENT"`
Expected: `removed`.

- [ ] **Step 5: Open the vault in Obsidian and spot-check that the graph/backlinks still resolve**

Manual: open Obsidian, open `20-projects/edbot-v2/edbot-v2.md`, confirm the four `[[...]]` links to context notes resolve (no broken-link styling). Wikilinks resolve by name, so this should be intact.

---

## Notes for the executor

- This plan has no automated test suite; "tests" are verification commands and one manual Obsidian check (Task 8 Step 5).
- If a `git mv` glob fails because a note name contains special characters, fall back to quoting the exact filename (Task 3 shows the quoted form).
- Frontmatter backfill (Task 5) is deliberately manual/per-file to avoid corrupting existing YAML — do not blind-`sed` across all files.
