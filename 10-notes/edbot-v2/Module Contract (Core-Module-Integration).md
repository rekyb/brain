---
title: Module Contract (Core-Module-Integration)
aliases: [Core Module Integration, Edbot Module Contract, Removable Modules]
tags: [edbot, architecture, modules, reference]
created: 2026-06-28
status: evergreen
source: "edbot-workspace/CLAUDE.md + roadmap specs E04/E06/E58 + PRDs E40/E45/E210 + testcases E08/E12/E13"
---

# 🧩 Module Contract — Core → Module → Integration

> [!abstract] TL;DR
> Both backend and frontend follow one rule: **Platform Core (protected) → Modules (self-contained, removable) → Integration (glue)**. A module imports **only** from `core/` and its own folder — never another module's internals. Cross-module talk goes through `core/events` (BE) or shared session stores (FE). The invariant that enforces it all: **deleting a module's folder + its registry line must still compile and run.** That removability is what lets ~8 people build epics in parallel without colliding.

Part of the [[Edbot v2]] cluster. This is the architectural heart.

## The one rule

```
Platform Core (protected)  →  Modules (self-contained, removable)  →  Integration (glue)
```

Removability is the load-bearing invariant: a module is "done right" only if you can `rm -rf` its folder + delete one registry line and the build still passes. CI greps + `module-loader.test.ts` back this; epic acceptance criteria (E08/E12/E33) bake "removability / contract intact" into the test matrix.

## Backend module anatomy (edbot-be)

Born by copying the template: `cp -r src/modules/_template src/modules/<id>`.

```
src/modules/<id>/
  manifest.ts          # id, basePath, gates — the module's identity card
  index.ts
  gates.ts             # GATES = { JOIN: "v2_<id>", ... } — named Statsig gates
  routes/<id>.routes.ts
  controller/ · service/ · repositories/ · models/
  config.schema.json   # JSON Schema for this module's config-engine payload (can be `{}`)
  README.md
  tests/               # incl. manifest-discovery smoke via module-loader.test.ts
```

**Loading = runtime filesystem scan.** `src/integration/module-loader.ts` scans `src/modules/*` (skipping `_template`/dotfiles), imports each `manifest.ts`, and **`manifest.id` MUST equal the directory name**. It mounts `routes/<id>.routes.ts` at `basePath ?? "/v2/<id>"` (+ any `legacyAliases`).

- Mount is `/v2/<id>` — **NOT** `/v2/modules/...` (e.g. `/v2/achievements/me`).
- **Adding a module → restart** (the scan happens at boot).
- Real example (E210 spec, verbatim): `manifest.id="program-engagement"`, `basePath="/v2/program-engagement"`, `GATES = { JOIN: "v2_program_engagement" }`, `config.schema.json` = `{}`.

## Frontend module anatomy (edbot-fe)

FE can't filesystem-scan — `output: "export"` forbids dynamic `import()`/`fs`. So registration is **static and explicit**.

```
src/modules/<id>/
  manifest.ts          # id, flag, status, route  (e.g. status:"experimental", flag:"v2_<id>")
  index.ts · meta.ts(.json)
  config.schema.json · README.md
  components/ · pages/ · hooks/ · services/ · schemas/ · tests/
  (often: experiments.ts, analytics.ts)
```

Registering a new FE module = **two lines**:
1. one import line in `src/integration/module-registry.ts`
2. a thin re-export at `src/app/<group>/<path>/page.tsx` (core owns the route tree; modules don't create routes directly).

**Visibility = two independent levers** (confirmed by E08/E12 "route pruned / ModuleGate fallback" tests):

| Lever | Mechanism | Effect |
| ----- | --------- | ------ |
| **Build-time** | manifest `status` | `status:"killed"` → pruned from the bundle entirely |
| **Runtime** | Statsig flag via `<ModuleGate>` | toggle a present module on/off without redeploy |

> [!info] Gate naming nuance
> The gate is *conventionally* `v2_<id>`, but a module declares gates in a `gates.ts` map and **can have several**. E40 search ships `v2_search_ui` (FE) and `v2_search_api` (BE) separately so UI and API toggle independently.

## Platform Core — the protected zone

Modules consume core; editing core requires flagging MED/HIGH risk and pausing for approval. Protected lists (from `CLAUDE.md`):

- **BE:** `src/core/`, `src/integration/`, `src/config/`, `src/app.ts`, `src/server.ts`, `src/modules/_template/`
- **FE:** `src/core/*`, `src/integration/*` (except adding a registry line), `src/app/layout.tsx` + route-group layouts

Core provides the four services a module leans on: **events** (`core/events`), **config** (`core/config-engine`), **feature flags** (`core/feature-flags` — FE reaches Statsig *only* through `@core/feature-flags`, never the SDK directly), and **auth/session**.

## Cross-module communication

- **BE — events.** Emit via `eventBus.publish({ type: "learning.lesson.completed", source: "learning", ... })`; others `subscribe` by pattern. Fire-and-forget → a removed module just means its handler never runs (no hard coupling). E04 is the spec; e.g. the `assessment` module subscribes to `learning.skills`.
- **FE — shared session stores**, read-only across modules. No `import { X } from '@/modules/other'`.

## config-engine — behavior without a deploy

Modules read config through `core/config-engine`, validated against the module's `config.schema.json`. Powerful pattern (E45, live): onboarding's `journeyStages` live in the config payload, each stage carrying an optional `flag` field — so product can **reorder/toggle onboarding steps from the panel**, with the in-code `DEFAULT_JOURNEY_STAGES` array as fallback when no payload exists.

**E58** adds safety: **draft → publish → rollback** with version snapshots (`module_configs` live + `module_config_versions` history), so a bad edit can't instantly break every config-driven module.

## feature-flags — Statsig, two-layer

- **Naming:** `v2_<module>` (`v2_dashboard`, `v2_search_ui`, `v2_program_engagement`), declared in `gates.ts`, seeded in `feature-flags.seed.json`, provisioned by `scripts/provision-statsig-gates.sh`.
- **Build-time** (`status:"killed"`) removes code; **runtime** (`<ModuleGate>` + Statsig) toggles a present module.
- FE imports flags only via `@core/feature-flags` — direct Statsig SDK use is on the FE "do NOT" list.

## The mental model in one line

> **Core is a small protected platform offering four services (events, config, flags, auth). Every feature is a new module that plugs in by convention — a manifest whose `id` is its folder name, routes auto-mounted at `/v2/<id>` (BE) or one registry line + a `<ModuleGate>` page (FE) — consumes only those four core services, talks to peers only through events, and can be deleted without breaking the build.**

> [!warning] Verification caveat
> Sections covering module anatomy, the dual gating, the config-engine pattern, and flags are cross-checked against specs that quote the real files (E40/E45/E210/E13). The deepest code-level details (exact `core/` subdirectory list, service signatures, collection schemas) are **reconstructed from references** — the app repos weren't on disk. Clone them (TEAM-WORKFLOW §1) to nail those down.

## Related

- [[Edbot v2]] · [[Edbot Workspace (Meta-Repo)]] · [[Edbot Roadmap & Epics]]
- Conceptual kin: [[Compound Components]] · [[Design Systems]] (same "small core + composable parts" philosophy)

## Sources

- `edbot-workspace/CLAUDE.md` — Architecture section, protected files, FE "do NOT" list
- `docs/roadmap/E06-platform-core-module-convention.md`, `E04-event-tracking-foundation.md`, `E58-config-draft-publish.md`
- PRDs: `docs/prd/e40-search-learn-assessment-prd.md`, `feature/E45-onboarding-skill-interest/plan.md`, `feature/E210-onboarding-program-step/plan.md`
- Testcases: `docs/testcases/e08-…`, `e12-…`, `e13-points-economy-redemption-testcases.md`
