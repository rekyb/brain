---
title: Edbot v2
type: moc
project: edbot-v2
aliases: [Edbot, Edbot.ai v2, edbot-workspace]
tags: [edbot, project, architecture, moc]
created: 2026-06-28
status: evergreen
source: "Study of edbot-workspace meta-repo (docs/, scripts/, CLAUDE.md, TEAM-WORKFLOW.md, admin-2026-prototype/)"
---

# 🤖 Edbot v2

> [!abstract] TL;DR
> **Edbot.ai v2** is a role-based, skill-verified, habit-driven **learning & employability platform** by Solve Education, built across **four git repos** coordinated by a **meta-repo** (`edbot-workspace`). A learner declares a livelihood goal → gets a role-aligned learning path → proves skills via assessments → earns verifiable credentials → sees a readiness radar of gaps. The whole thing runs on a strict **[[Module Contract (Core-Module-Integration)|removable-module architecture]]**, ships everything behind Statsig feature flags, and tracks all work in ClickUp.

Hub note (MOC) for the Edbot v2 cluster. This is the map; the spokes go deep.

## The cluster

| Note | Covers |
| ---- | ------ |
| [[Edbot Workspace (Meta-Repo)]] | The coordination layer — 4 repos, `nb`/`sb`/`bi` tooling, `local-main`, ClickUp, branching workflow |
| [[Module Contract (Core-Module-Integration)]] | The architecture: Platform Core → Modules → Integration, removability, events, config-engine, feature flags |
| [[Edbot Roadmap & Epics]] | Phases (NOW/NEXT/LATER), motivational levers, the epic (Exx) system, PRD/ADR/testcase docs |
| [[LEDGER Admin Prototype]] | The keyboard-first admin-panel redesign prototype (the only live code on disk) |

## What the product is

An education + employment-readiness platform aimed at underserved markets (**6 locales**: EN, ID, MS, HI, TH, KM). Core loop:

> Declare a **livelihood goal** ("become a freelance designer") → take a **role-aligned learning path** → **prove skills** via assessments → earn **verifiable credentials** → see a **readiness radar** of gaps vs. the target role — motivated by **streaks / XP / social proof**.

**User roles:** Learner · Sponsor · Expert (content author) · Admin.

**Feature domains:** Learn · Prove (assessment + skill radar) · Earn (XP / leaderboards / challenges) · Habits (streaks + freeze/recovery) · Identity & Onboarding · Credentials · Search · Localization.

## The four repos

| Repo | Folder | Stack | Role |
| ---- | ------ | ----- | ---- |
| **edbot-be** | `edbot-be/` | Express · TS · MongoDB · Redis | Backend API + all business modules |
| **edbot-fe** | `edbot-fe/` | Next.js · TS · MobX · Tailwind | Learner-facing UI (`output: "export"`) |
| **edbot-panel** | `edbot-panel/` | Next.js 14 · TS | Admin control plane |
| **edbot-qa** | `edbot-qa/` | Playwright · TS | API + e2e regression |

`edbot-be` + `edbot-fe` move in lockstep as one feature (shared branch names); panel and QA evolve on their own cadence. See [[Edbot Workspace (Meta-Repo)]].

## How it all fits

- **Architecture** is the foundation: every feature is a self-contained, removable module plugged into a small protected core → [[Module Contract (Core-Module-Integration)]].
- **Process** wraps it: epics (Exx) flow PRD → branch → `local-main` integration → MR → ship Friday, tracked in ClickUp → [[Edbot Workspace (Meta-Repo)]] + [[Edbot Roadmap & Epics]].
- **Everything is flagged** (Statsig `v2_*`) and **config-driven** (admin-editable config-engine), so product can reshape and toggle modules at runtime.

> [!note] Scope of these notes
> Written from studying the **`edbot-workspace` meta-repo** (docs + tooling + the admin prototype). The actual `edbot-be`/`fe`/`panel` source was **not on disk** when these were written — code-level details are grounded in the roadmap specs, PRDs, and testcases that cite the real files. Where a detail is reconstructed rather than documented, the spoke notes say so.

## Related

- [[Module Contract (Core-Module-Integration)]] · [[Edbot Workspace (Meta-Repo)]] · [[Edbot Roadmap & Epics]] · [[LEDGER Admin Prototype]]
- Design background: [[Design Systems]] · [[Atomic Design]]

## Sources

- `edbot-workspace/CLAUDE.md` — architecture contract, branching, protected files
- `edbot-workspace/TEAM-WORKFLOW.md` — team process, lifecycle, ClickUp integration
- `edbot-workspace/docs/roadmap/` — epic specs · `docs/prd/` — PRDs · `docs/adr/` — decisions
