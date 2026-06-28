---
title: LEDGER Admin Prototype
aliases: [LEDGER, admin-2026-prototype, Edbot Admin Redesign]
tags: [edbot, prototype, ux, design-systems, frontend]
created: 2026-06-28
status: evergreen
source: "edbot-workspace/admin-2026-prototype/ (README, src/)"
---

# 📟 LEDGER — Admin 2026 Prototype

> [!abstract] TL;DR
> **LEDGER** is the only live application code in the [[Edbot Workspace (Meta-Repo)|workspace]] on disk — a standalone Next.js 14 prototype reimagining **edbot-panel** as a **keyboard-first, dense "Bloomberg-terminal-meets-code-editor"** admin tool. Core insight: **the table _is_ the interface.** Warm dark, one amber accent, JetBrains Mono for all data so columns align to the pixel. 100% mock data — it's a UX/interaction prototype meant to be wired to real panel APIs later.

Part of the [[Edbot v2]] cluster — a design exploration for the admin control plane.

## The thesis

"What should a dense-data admin tool feel like when an operator lives in it 6 hours a day?" Answer: stop wrapping data in chrome and make the data the entire surface. Key shifts from a traditional admin panel:

- **No left sidebar** → a 44px top rail with saved-view tabs + command palette + `g`-chord jumper (≈240px more data per screen).
- **No modals** → records slide in as a **side-peek** over a dimmed-but-live table; walk a queue with `J`/`K` without losing place.
- **No "Are you sure?"** on reversible actions → optimistic + deep undo stack; only irreversible verbs get typed confirmation.
- **Density as a toggle** (`⇧D` cycles 28/34/40px rows) + hairline-only ruling → 30–40% more rows visible.
- Full **mouse parity**, but a fluent operator never needs the mouse.

## What's built

| Screen | Route | Purpose |
| ------ | ----- | ------- |
| Overview | `/` | Control-plane heartbeat — metric band (6 KPIs + sparklines), module/queue strips, triage queue with act-and-advance verbs. |
| Learners | `/learners` | Flagship dense-table demo — 64 seeded learners, saved views, 10-col layout, filter composer, side-peek edit form. |
| Domain stub | `/[domain]` | Graceful zero-state for the 12 unbuilt domains. |

The star component is `ledger-table.tsx` — keyboard nav (`j`/`k`/`J`/`K`), selection (`x`, `⇧J/K`, `⌘A`), filter pills (`f` → `field:value` with schema autocomplete), single-key verbs, side-peek (`↵`), density toggle, undo/redo. `console-provider.tsx` runs the global keyboard layer (`⌘K` palette, `?` cheatsheet, `g<key>` jumper, `U`/`⌘Z` undo).

## Design language

- **Stack:** Next.js 14.2 · React 18 · TypeScript (strict) · Tailwind 3.4 · lucide-react. **No component library** — built from scratch.
- **Type duality (the signature):** Hanken Grotesk for UI chrome; **JetBrains Mono** (tabular figures) for every datum so columns align. Nothing larger than 24px.
- **Color:** near-black canvas, layered surfaces, hairline dividers, and **one molten-amber accent** (`#F5B312`) used *only* for focus/active/selection — "an LED, never prose." Status colors (success/warning/danger/info) for state only, never buttons.
- **Motion confirms causality, never decorates:** row-move + focus-ring at **0ms**, side-peek slide 240ms, commit-flash 200ms; respects `prefers-reduced-motion`. Data is never boxed (only ruled); square corners on data, rounded only on interactive chrome.

## Data model it implies

A **Learner** entity with ~25 fields: identity (id/handle/email), lifecycle (status, joined/last-active), engagement (xp, level, streakDays, riskScore, weeklyMinutes), localization (region/locale/tz/device), cohort/plan/referral, guardian + compliance (guardianEmail/verified, marketingConsent), and operator metadata (flags[], notes). Dashboard mocks 6 KPIs, 8 modules (with rollout %/state/p95/error-rate), 4 operator queues, and a system-events log.

> [!note] Status
> **100% mock data** (deterministic seeded PRNG). This is an interaction prototype, not an integration demo. The README's adoption path: replace the panel's shell with `TopRail` + `ConsoleProvider`, extend its command palette with sigil routing, and wrap each domain's list page in `LedgerTable` with that domain's column/filter/verb config.

## Related

- [[Edbot v2]] · [[Edbot Workspace (Meta-Repo)]]
- Design lineage: [[Design Systems]] · [[Design Tokens]] · [[UI-UX Best Practices]] · [[Accessibility Essentials]]

## Sources

- `edbot-workspace/admin-2026-prototype/README.md`
- `src/app/` (page, learners, [domain]) · `src/components/` (ledger-table, console-provider, top-rail, peek-form, overview-band) · `src/lib/mock/`
