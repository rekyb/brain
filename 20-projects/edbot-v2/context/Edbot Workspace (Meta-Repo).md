---
title: Edbot Workspace (Meta-Repo)
aliases: [edbot-workspace, Edbot Meta-Repo, Edbot Tooling]
tags: [edbot, workflow, tooling, git, reference]
created: 2026-06-28
status: evergreen
source: "edbot-workspace/CLAUDE.md + TEAM-WORKFLOW.md + scripts/ + INTEGRATION-CONFLICTS.md"
---

# 🛠️ Edbot Workspace (Meta-Repo)

> [!abstract] TL;DR
> `edbot-workspace` is **not an application** — it's a **coordination meta-repo** that tracks only shared tooling + docs. The four real app repos live *inside* it but are their own git repos (gitignored here). `direnv` puts `scripts/` on PATH, giving multi-repo commands (`nb`, `sb`, `bs`, `bi`, `wpush`…) that operate across all repos at once. Work is tracked in **ClickUp** (not markdown — that caused merge conflicts); features ship trunk-based behind flags via per-repo MRs to `main`.

Part of the [[Edbot v2]] cluster — the *process & tooling* layer wrapping the [[Module Contract (Core-Module-Integration)|architecture]].

## What's actually on disk

The meta-repo holds: `scripts/` (the multi-repo CLI), `docs/` (roadmap, PRDs, ADRs, testcases), guidance (`CLAUDE.md`, `TEAM-WORKFLOW.md`), `feature-flags.seed.json`, and one prototype ([[LEDGER Admin Prototype]]). The four app repos are **gitignored and cloned in separately** per teammate (`TEAM-WORKFLOW.md §1`) — they are *not* nested into the meta-repo.

> [!tip] Commit scope
> App code commits go **inside each app repo** (commit BE + FE separately with matching messages). The meta-repo only tracks shared tooling + guidance — never app code.

## The multi-repo CLI (`scripts/`)

All commands iterate over the four repos. `.envrc` does `PATH_add scripts` + sources `.envrc.local` (secrets).

| Command | What it does |
| ------- | ------------ |
| `nb <branch>` | New matching branches (BE+FE default; `--all` adds panel; QA opt-in via `--repos`). Validates **all** repos before mutating any (atomic). |
| `sb <branch>` | Switch to a branch only in repos that have it (scope-aware). |
| `bs` | Show current branch + dirty count per repo; **warns on BE/FE mismatch**. |
| `wst` / `wpull` / `wpush` / `wlog` | Bulk git status/pull/push/log across all repos. |
| `bi` | Rebuild the disposable `local-main` integration branch from the manifest. |
| `clickup-epic claim\|progress\|done\|blocked\|add-test <Exx>` | Track work status in ClickUp. |
| `gen-leaderboard` | Parse git history across repos → `LEADERBOARD-CONTRIBUTION.md`. |
| `sonar-check` | Pre-push SonarQube quality-gate guard. |

## `local-main` + the integration manifest

The pattern that lets many in-flight epics be tested together **before any merges to `main`**:

- `scripts/integration.manifest` lists `<repo> <branch>` lines, in **dependency/merge order**.
- `bi` rebuilds **`local-main` = `origin/main` + each manifest branch, merged in file order**, *fresh every run* — so it can't rot, and anyone with the manifest reproduces the identical integration.
- `local-main` is **disposable**: never push it, never build real work on it (a rebuild discards it). Each feature still ships via its **own MR to `main`** (trunk-based + feature-flagged — **not** Git Flow).
- Add a manifest line when you start integrating; **delete it once your MR lands on `main`**. Empty manifest ⇒ `local-main == main`.
- On conflict, `bi` aborts and names the offending branch. `INTEGRATION-CONFLICTS.md` is a worked example of drift when `origin/main` advances past in-flight branches → owners rebase.

## Branching workflow

- Branches: `feature/Exx-desc`, `bug/Exx-desc`, `improve/Exx-desc`, `chore/desc`.
- **`main` is MR-only** — never push/force-push directly. A pre-commit hook prints the branch and blocks direct commits to `main`/`dev`.
- FE branches base on `main`, **not `dev`** (`dev` is ~120 commits stale).
- Cross-repo dependency lives in the **manifest's merge order**, not in branch names (same names are just coordination ergonomics).

> [!warning] One active task per repo checkout
> Two agents on the same folder, different branches, **will collide** — one switches the branch out from under the other and commits land on the wrong branch (this actually happened once). For parallelism in one repo, use a `git worktree` / `isolation: worktree`. Verify the branch before **every** commit (`bs`).

## ClickUp = source of truth for tracking

Tracking moved off markdown (roadmap/`STATUS.md`/`CHANGELOG.md` are **deprecated** as live trackers — they caused cross-machine merge conflicts). Three automation layers:

1. **Git hooks** (`clickup-hook` via `install-hooks`) — auto-bump a task to *In Progress* on first commit/checkout of a `feature/eNN-*` branch (never downgrades).
2. **`clickup-epic` script** — claim / progress / done / blocked / add-test, each with a dated comment.
3. **ClickUp MCP** — Claude can update tasks from chat.

Every epic is a task `[Exx] Title` in the **Edbot v2 List**. PRD/plan working files still live in-repo at `docs/prd/<branch-name>/` (the `/build` inputs); only *tracking* lives in ClickUp.

## Team shape

~8 product engineers + 2 QA (per `LEADERBOARD-CONTRIBUTION.md`). Weekly cadence: pick → claim → branch → build behind a flag → integrate via `bi` → MR → **deploy every Friday**. One owner per epic; dependencies made explicit via the manifest + per-epic spec contract.

## Related

- [[Edbot v2]] · [[Module Contract (Core-Module-Integration)]] · [[Edbot Roadmap & Epics]]

## Sources

- `edbot-workspace/CLAUDE.md` — branching workflow, `local-main`, concurrent-agent rules
- `edbot-workspace/TEAM-WORKFLOW.md` — full team process, lifecycle, ClickUp setup
- `scripts/` — `branch.sh`, `build-integration.sh`, `clickup-epic`, `clickup-hook`, `gen-leaderboard`
- `INTEGRATION-CONFLICTS.md` — worked `bi` conflict-resolution example
