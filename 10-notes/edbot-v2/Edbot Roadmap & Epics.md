---
title: Edbot Roadmap & Epics
aliases: [Edbot Roadmap, Edbot Epics, Phases and Levers]
tags: [edbot, roadmap, product, process, reference]
created: 2026-06-28
status: evergreen
source: "edbot-workspace/docs/roadmap/ + docs/prd/ + docs/adr/ + docs/features/ + docs/testcases/"
---
# 🗺️ Edbot Roadmap & Epics

> [!abstract] TL;DR
> Work is organized as **epics** (`Exx`, E01 → E280+), each tagged with a **Phase** (NOW → NEXT → LATER) and a motivational **Lever**. Every epic gets a roadmap spec (what/why/contract), a per-branch **PRD** (`docs/prd/<branch>/`), and **testcases** (Gherkin → Playwright). Decisions are captured as **ADRs**. The roadmap markdown is now *history* — live tracking lives in ClickUp (see [[Edbot Workspace (Meta-Repo)]]).

Part of the [[Edbot v2]] cluster — the *what & why* layer.

## Phases

A three-phase arc (from the strategy doc):

| Phase | Goal | Examples |
| ----- | ---- | -------- |
| **NOW** | Fix the motivation foundation — identity, tiny habits, quick wins; lay data + dev-infra | E01 identity, E02 goal-linked streak, E03 quick-win, E04 events, E05 taxonomy, E06 platform core, E07 panel, E26 habit engine |
| **NEXT** | Build engagement & role-based progression | E08 credentials, E11 analytics, E25 content authoring, E33 skill radar, E34 role catalog, E40 search, E43 skill detail |
| **LATER / Cache** | Operational & scalability work | E100–E124 Redis caching layer, perf/security tunings |

## Levers

Motivational/product domains (a GAIN-style framing, Fogg/Clear influence) that each epic is tagged with:

**Motivation · Ability · Trigger · Data Layer · Foundation · Livelihood · Discovery · Trust · Reach · Activation/Operability**

Each epic header reads like: `> Phase: NOW · Lever: Motivation · Status: ✅ Done`. The per-repo sections of a spec make explicit **which repos** a build touches and what "done" looks like for each.

## The epic system

- **IDs keep the `Exx` scheme**; the ClickUp task name is prefixed `[Exx]` so branch names (`feature/Exx-*`) and cross-references resolve.
- **Slices → sub-tasks.** A PRD's vertical slices each become a ClickUp sub-task.
- **Spec anatomy:** phase/lever/repos/deps header → Goal → Why → per-repo (current state → gap → scope → acceptance criteria) → Dependencies.

Representative epics: E01 identity onboarding · E08 skill-verification credentials · E26 habit engine (streak freeze/recovery) · E33 skill-radar gap · E40 search-learn-assessment (clean dual-flag BE/FE example) · E58 config draft/publish · E125 dynamic RBAC.

## The doc system (`docs/`)

| Dir | Holds |
| --- | ----- |
| `docs/roadmap/` (~291 files) | Per-epic `Exx-*.md` specs. Deprecated as *live tracker*, rich for history. |
| `docs/prd/<branch>/` | Per-epic PRDs (Draft → In Review → Approved → In Progress → Shipped), ~150 across `feature/ bug/ improve/ test/`. |
| `docs/adr/` | Architecture Decision Records — currently **one** (0001: persist guest profiles via v1's HTTP API, not a v2 collection). One ADR ⇒ still a modular monolith, not distributed. |
| `docs/features/` | e.g. `assessment-integrity.md` (E23 anti-cheat behavioral signals). |
| `docs/reviews/` | e.g. a 13-persona (5 learner + 8 accessibility) onboarding walkthrough with finish/abandon verdicts. |
| `docs/testcases/` | 11 Gherkin suites, each with a traceability matrix (PRD AC → test ID → Playwright `file:line`). |

`MANUAL-TEST-STATUS.md` is QA's staging readiness board (notes in Bahasa Indonesia); `CHANGELOG.md` is a per-epic behavior log written by `/done` on merge.

## PRD lifecycle

```
Draft → In Review → Approved → In Progress → Shipped
```

`cp docs/prd/_template.md …` → open a workspace MR (`prd(Exx): …`) → merge = team contract → build against acceptance criteria → set `status: Shipped` on merge. Hotfixes/chores skip the PRD.

> [!note] Path override
> Workflow skills that default to `.claude/artifacts/` or `*-plan.md` must write to `docs/prd/<branch-name>/` instead (named exactly `plan.md`/`context.md`) — the defaults are gitignored in the workspace and would hide work from teammates.

## Related

- [[Edbot v2]] · [[Edbot Workspace (Meta-Repo)]] · [[Module Contract (Core-Module-Integration)]]

## Sources

- `docs/roadmap/README.md` + `docs/roadmap/Exx-*.md` — board + epic specs
- `docs/prd/README.md`, `_template.md` — PRD process
- `docs/adr/0001-guest-profile-via-v1-http.md` — the one decision record
- `docs/testcases/`, `docs/reviews/v2-onboarding-personas/`, `docs/features/assessment-integrity.md`
