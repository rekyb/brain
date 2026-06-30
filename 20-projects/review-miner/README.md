# review-miner

A product-agnostic Python CLI that **collects** competitor product reviews
(App Store, Google Play, Reddit, G2, Capterra) into a single clean CSV for fast,
manual pre-launch market research. It collects and normalizes — it does **not**
analyze; synthesis happens afterward in Claude/ChatGPT.

## Documents

| File | Purpose |
|------|---------|
| [PRD.md](PRD.md) | Product Requirements — problem, users, scope, success criteria, user stories & acceptance criteria |
| [DESIGN-SPEC.md](DESIGN-SPEC.md) | Technical design — architecture, connectors, data model, tech stack, alternatives considered |

## Status

- ✅ Brainstorming + design spec approved
- ✅ PRD drafted (v0.1, 2026-06-30)
- ⏳ Implementation — to be wired to an implementation agent (built code will live here under `20-projects/review-miner/`)

## For the implementation agent

Build against **DESIGN-SPEC.md** (the *how*) and **PRD.md** (the *what* + acceptance criteria).
Resolve the Open Questions in PRD §10 with the product owner before/at build start.
