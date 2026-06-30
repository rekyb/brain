# Product Requirements Document (PRD) — review-miner

> Generated from the brainstorming session and design spec
> (`docs/superpowers/specs/2026-06-30-review-miner-design.md`).
> This is an **internal tooling PRD** for a product-research utility, so
> market-sizing and revenue sections are framed for internal value, not external
> sales. Gaps are tagged inline: 🔶 Assumption (plausible, unvalidated) /
> 🔵 Open Question (unknown, needs a decision).

## Document Information

### Authors

- rekybongso@gmail.com

### Reviewers

- 🔵 Open Question: who else should review before the implementation agent is wired up?

**Date:** 2026-06-30

### Change Log

| Version | Date       | Author    | Change Description |
|---------|------------|-----------|--------------------|
| 0.1     | 2026-06-30 | Product   | Initial draft from design spec |

***

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Problem Statement](#2-problem-statement)
3. [Target Users & Personas](#3-target-users--personas)
4. [Strategic Context](#4-strategic-context)
5. [Solution Overview](#5-solution-overview)
6. [Success Metrics](#6-success-metrics)
7. [User Stories & Requirements](#7-user-stories--requirements)
8. [Out of Scope](#8-out-of-scope)
9. [Dependencies & Risks](#9-dependencies--risks)
10. [Open Questions](#10-open-questions)

***

## 1. Executive Summary

We're building **review-miner**, a product-agnostic Python CLI, for **time-constrained product managers running pre-launch market research** to solve the problem that **high-touch UX research is too slow and expensive before a launch decision**, which will result in **a reusable tool that collects honest, at-scale voice-of-customer data (competitor reviews) into a single clean CSV in minutes per project, so the manual synthesis can happen immediately in Claude/ChatGPT.**

The tool's scope is deliberately narrow: it **collects and normalizes** reviews. It does **not** analyze them — clustering, sentiment, and synthesis stay with the user. This boundary keeps the tool small, cheap, and reusable across projects.

***

## 2. Problem Statement

### Who Has This Problem?

Product managers and founders (starting with the Solve Education product team) who need to understand a market **before** committing to a product launch, but cannot afford the time or cost of high-touch UX research (recruiting, scheduling, moderated interviews, synthesis).

### What Is the Problem?

Before launch, the fastest honest signal about a market lives in **existing competitor reviews** (App Store, Google Play, G2, Capterra, Reddit). But mining them by hand is slow and tedious: opening each platform, reading and copying reviews, normalizing wildly different formats, and de-duplicating — repeated from scratch for every new project. The collection grind eats the time that should go into *analysis*.

### Why Is It Painful?

- **User impact**: Hours of manual copy-paste per project before any thinking can begin; inconsistent coverage; the research often gets skipped under deadline pressure, so launch decisions are made on gut feel.
- **Business impact**: Launch decisions made with weak market evidence → higher risk of building the wrong thing. 🔶 Assumption: the team currently under-invests in pre-launch review research specifically because the collection step is manual.

### Evidence

- **Conversation origin**: This tool was scoped directly from a product-research planning discussion (2026-06-30) on how to research a market fast without high-touch UX research; review mining was identified as the highest-ROI scrappy method.
- 🔵 Open Question: how many hours/project does manual review collection currently cost the team? No baseline measured.
- 🔵 Open Question: how many launches in the last year skipped review research due to time? Unknown.

***

## 3. Target Users & Personas

### Primary Persona

- **Name**: Reky (the operator)
- **Role**: Product lead doing pre-launch market research across multiple initiatives
- **Context**: Works in an Obsidian/markdown knowledge vault under git; comfortable running a Python CLI; uses Claude/ChatGPT for analysis; time-constrained, juggling several projects
- **Goals**: Get a clean, consistent dataset of competitor reviews for *any* new project quickly, then do the thematic thinking themselves
- **Pain points**: Manual collection is slow and repetitive; each platform formats data differently; no reusable workflow across projects
- **Current behaviour**: Either does ad-hoc manual review reading or skips structured review research entirely under deadline

### Secondary Personas (if applicable)

- **Name**: A future implementation/automation agent
- **Role**: The coding agent the user will "wire later" to build and run the tool
- **Differs from primary**: Consumes the PRD + design spec as machine-readable requirements rather than operating the CLI interactively

### Jobs-to-Be-Done

- **Functional job**: "When I start researching a new market, I want to gather all relevant competitor reviews into one clean file fast, so I can spend my time analyzing instead of collecting."
- **Emotional job**: Feel confident the launch decision rests on real customer voice, not guesswork — without the dread of a manual slog.
- **Social job**: Be seen by the team as someone who makes evidence-based launch calls quickly. 🔶 Assumption (inferred, not stated).

***

## 4. Strategic Context

### Business Goals

- **Strategic alignment**: Faster, evidence-based pre-launch decisions across the product portfolio. The tool is reusable infrastructure, so its value compounds across every future project.
- **Revenue impact**: Indirect — better launch decisions reduce wasted build effort. 🔵 Open Question: is there a target number of launches/quarter this should support?

### Market Opportunity (optional)

- Not applicable in the traditional sense — this is an internal tool, not a market-facing product. Its "market" is the team's own portfolio of launch decisions.

### Competitive Landscape

- **Current alternatives**: Manual review reading; paid scraping vendors (Apify, Bright Data, SerpApi); low-code platforms (n8n); all-in-one review-analytics SaaS.
- **Our gap / why build vs. buy**: Off-the-shelf options either cost per-run, lock data into their format, or (n8n) can't host the two hard parts — Playwright scraping of JS-heavy sites and the clean Python collection libraries. A small owned CLI gives full control, vault-native output, and zero per-run vendor cost. (n8n was explicitly evaluated and set aside — see design spec "Alternatives considered.")

### Why Now?

- The need surfaced from an active pre-launch research question. Building the reusable collector now means every subsequent project benefits, rather than repeating manual collection each time.

***

## 5. Solution Overview

### Solution Description

review-miner is a product-agnostic Python CLI. On `review-miner run`, an **adaptive interactive wizard** asks which project, which sources, and the source details (app IDs, subreddits, competitor review URLs) — only prompting for the sources selected. Answers are validated, **saved to a per-project config**, and reused on later runs ("Reuse <project> config? [Y/n]").

The tool then calls each selected source's **isolated connector**, normalizes every source's fields onto one shared schema, de-duplicates, and writes a **CSV plus a markdown run-summary** into that project's vault folder. A failing connector (markup change, IP block) logs a warning and is skipped — the run always completes with whatever was collected. Analysis happens afterward, manually, in Claude/ChatGPT.

### Key Features

- **Product-agnostic, config-driven**: reusable across projects; a new project is a short wizard run, not code changes.
- **Adaptive wizard + ask-then-save config**: dynamic prompts that adapt to selected sources; answers persisted for one-keystroke reruns.
- **5 isolated connectors**: App Store, Google Play, Reddit (clean libraries/API) + G2, Capterra (Playwright DIY scrape).
- **Graceful per-source failure**: one broken connector never fails the whole run; failures are visible in the run-summary.
- **One normalized CSV + markdown run-summary** written into the vault per project.

### User Flows / Wireframes (optional)

- See terminal-flow walkthrough in the design spec (§ User flow).

### Story Map (optional)

- Design spec: `docs/superpowers/specs/2026-06-30-review-miner-design.md`

***

## 6. Success Metrics

> 🔵 Open Question: no baselines exist yet — these targets are proposed, not validated. Confirm before treating as commitments.

### Primary Metric

- **Metric**: Time-to-dataset — minutes from `review-miner run` to a usable CSV for a project
- **Current**: 🔶 Assumption: manual collection currently takes hours per project (unmeasured)
- **Target**: 🔶 Assumption: < 15 minutes of active time per project
- **Timeline**: Measured on first 3 real project runs after launch

### Secondary Metrics

- **Reuse**: number of distinct projects the tool is run against (proxy for product-agnostic value)
- **Coverage**: reviews collected per run across sources
- **Reliability**: % of runs where all selected connectors succeed (no silent scrape failures)

### Guardrail Metrics

- **Data quality**: duplicate rate in output CSV stays low (dedupe working); normalized schema holds across all sources
- **Maintenance burden**: 🔶 Assumption — scraper breakage shouldn't require more than occasional single-file patches (the DIY-scraping risk)

***

## 7. User Stories & Requirements

### Epic Hypothesis

We believe that a product-agnostic CLI that collects competitor reviews into a clean CSV will let the product team run pre-launch review research in minutes instead of hours, because the manual collection grind is the main thing that makes review mining get skipped. We'll measure success by time-to-dataset and by the tool being reused across multiple projects.

### User Stories

**Story 1: Configure a run via an adaptive wizard**

As a PM, I want the tool to ask me what to collect when I run it, so that I don't have to hand-write a config file.

**Acceptance Criteria:**

- [ ] `review-miner run` prompts for a project name
- [ ] User selects sources from a multi-select (App Store, Google Play, Reddit, G2, Capterra)
- [ ] The wizard only asks follow-up questions for the sources selected
- [ ] Inputs are validated before any network call; invalid input (e.g. empty app-ID list) is rejected at the prompt with a clear message

**Story 2: Save and reuse per-project config**

As a PM re-mining a project, I want my answers remembered, so that I don't retype IDs and URLs every run.

**Acceptance Criteria:**

- [ ] On first run, answers are written to `config/<project>.yaml`
- [ ] If a config for the named project exists, the tool offers "Reuse <project> config? [Y/n]"
- [ ] Choosing "Y" skips all remaining prompts and runs; "n" re-enters the wizard

**Story 3: Collect reviews from selected sources into a normalized dataset**

As a PM, I want reviews from all selected sources merged into one consistent file, so that I can analyze them in one place.

**Acceptance Criteria:**

- [ ] Each source's connector returns reviews mapped onto the shared schema (`product, source, review_id, date, rating, title, body, author, url, helpful_count, language, scraped_at`)
- [ ] Reviews are de-duplicated (by source-native id + body hash)
- [ ] `max_per_source` limits how many reviews each source returns
- [ ] App Store / Google Play / Reddit use their clean libraries/API; G2 / Capterra use Playwright

**Story 4: Survive source failures without losing the run**

As a PM, I want a failed source to not kill the whole run, so that I still get the data that did come through.

**Acceptance Criteria:**

- [ ] A connector that errors (markup change, IP block, timeout) logs a warning and returns what it has
- [ ] The run completes and writes output even if one or more connectors failed
- [ ] The run-summary reports per-source success/failure and counts (e.g. `G2: 0 ✗ blocked`)

**Story 5: Write outputs into the vault**

As a PM, I want results saved into my project's vault folder, so that they live alongside my notes and in git.

**Acceptance Criteria:**

- [ ] Writes `reviews_<date>.csv` to the project's `output_dir`
- [ ] Writes `run-summary_<date>.md` with date, config used, and per-source counts
- [ ] Default `output_dir` is `20-projects/<project>/research/review-mining/`

### Constraints & Edge Cases

- **Technical constraints**: Python 3.11+; Reddit requires API credentials in `.env`; Playwright requires a headless browser install; politeness (sleep + jitter, rotating user-agents) on scraped sites.
- **Edge cases**: a selected source has zero results; duplicate reviews across runs; missing `rating` (some Reddit posts) → null; Reddit creds missing when Reddit is selected → warn before run.

***

## 8. Out of Scope

### Not Included in This Release

- **Any analysis / sentiment / clustering / summarization** — deliberately the user's manual step in Claude/ChatGPT. This is the core scope boundary.
- **Scheduling / continuous monitoring** — run on demand only. (Could be added later via n8n calling the CLI — see design spec.)
- **A database** — CSV files on disk are the storage.
- **Incremental "only-new-since-last-run" pulls** — each run is a fresh full pull.
- **Dashboard / GUI** — CLI only.

### Future Considerations

- LLM-assisted theming step (was explicitly declined for v1; user prefers manual analysis).
- Additional connectors (Trustpilot, Amazon, YouTube comments).
- n8n orchestration layer for scheduled/recurring runs.

***

## 9. Dependencies & Risks

### Dependencies

- **Technical**: Python 3.11 + `uv`; libraries `app-store-scraper`, `google-play-scraper`, `praw`, `playwright`, `pandas`, `typer`, `questionary`, `pydantic`. Playwright browser binaries must be installed.
- **External**: Reddit API credentials (free app registration → `.env`). Third-party site structure for G2/Capterra (uncontrolled).
- **Team**: An implementation agent to be wired by the user to build the tool from this PRD + design spec.

### Risks & Mitigations

- **Risk (feasibility/maintenance)**: G2/Capterra DIY scrapers break when sites change markup or block IPs.
  - **Mitigation**: Isolate scraping to two files; fail gracefully + report in run-summary; politeness throttling. Accepted, user-chosen tradeoff.
- **Risk (viability/legal)**: Scraping G2/Capterra is ToS-sensitive.
  - **Mitigation**: Documented as an accepted tradeoff in the design spec; internal/low-volume use; isolated so it can be disabled per-project.
- **Risk (value)**: Tool is built but review mining still gets skipped, or output isn't actually faster to analyze.
  - **Mitigation**: 🔵 Open Question — validate time-to-dataset on the first real project before investing in more connectors.
- **Risk (usability)**: Wizard friction or Playwright setup deters reuse.
  - **Mitigation**: ask-then-save config; clear `.env.example`; documented setup.

### Cagan's Four Risks summary

- **Value**: 🔶 Assumption that faster collection unlocks more/better review research — unvalidated.
- **Usability**: low risk for a technical primary user.
- **Feasibility**: medium — Playwright scraping is the brittle surface, contained by design.
- **Viability**: low cost; main concern is ToS, mitigated above.

***

## 10. Open Questions

| Question | Owner | Deadline | Status |
|----------|-------|----------|--------|
| What is the current baseline time/project for manual review collection? | Product | Before metrics are treated as commitments | Open |
| Who reviews this PRD before the implementation agent is wired? | Product | Before build | Open |
| Are the proposed success targets (<15 min, reuse count) acceptable? | Product | Before launch eval | Open |
| Which projects/competitors will be the first real test of the tool? | Product | First run | Open |
| Is G2/Capterra scraping acceptable for the team's risk tolerance, or restrict to API-clean sources only? | Product | Before build | Open |

***

## PRD Self-Assessment

### Strongest Section

- **Section 5 — Solution Overview** and **Section 7 — User Stories**. These come straight from an approved, detailed design spec, so the *what* and *how* are well-specified and engineering-ready.

### Weakest Section

- **Section 6 — Success Metrics**. No baselines exist; targets are assumptions. The tool's *value hypothesis* (faster collection → more/better research → better launch decisions) is plausible but unvalidated.

### Top Assumptions to Validate

| # | Assumption | Section | Risk if Wrong | Proposed Validation |
|---|------------|---------|---------------|---------------------|
| 1 | Manual review collection currently costs hours/project and is the reason review research gets skipped | 2, 6 | The tool solves a non-problem; effort wasted | Quick retro/time-log on the next manual collection before building |
| 2 | DIY scraping of G2/Capterra is maintainable with occasional single-file patches | 6, 9 | Ongoing maintenance burden outweighs value | Build G2 connector first as a spike; observe breakage rate |
| 3 | Faster collection translates into actually doing more/better pre-launch research | 4, 9 | Tool is built but behavior doesn't change | Run on the first real project; check whether analysis followed |

### Recommended Next Step

- Confirm the **open questions in Section 10** (especially the G2/Capterra scraping risk tolerance and the first test project), then hand this PRD + the design spec to the implementation agent. The design spec already contains the technical architecture; this PRD provides the problem framing, personas, scope, and acceptance criteria to build against.

***

*Generated 2026-06-30 from the review-miner brainstorming session and design spec.*
