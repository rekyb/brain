---
title: book-tracker-validation-research-plan
aliases: [book tracker research questions]
type: spec
project: book-tracker
status: active
created: 2026-07-09
updated: 2026-07-09
tags: [research, validation, competitive-analysis]
source:
---

# Book tracker — validation research plan

> [!abstract] Goal
> Validate (or kill) the book-tracker idea via **secondary research only**: competitor benchmarking + review mining. Output: answered questions below, a filled feature matrix, and a wedge decision.

## Background

Relies on: [[book-tracker-competitive-landscape]] (priors to test), [[Free App Store Keyword Research]] (method).

**Benchmark set**: Goodreads, StoryGraph, Bookly, Basmo, Bookmory, Reading List, + Fable or Hardcover as the social control.

**Suggested order**: A (an afternoon) → C (richest per hour) → E → B/D fall out of review notes along the way.

## Research questions

### A. Demand & market shape *(store listings, category ranks, download proxies)*

- [ ] A1. How big is active demand? Download/rating counts of the top 10 apps for "reading tracker" / "book tracker". Are any apps < 2 yrs old gaining traction? (New entrants gaining = market accepts newcomers.)
- [ ] A2. Which search terms do incumbents target in titles/subtitles? Does anyone own "reading streak" or "TBR" in a title? (Gaps = ASO openings.)
- [ ] A3. Is demand growing or flat? Google Trends 5-yr on "reading tracker app", "goodreads alternative", "TBR app". Did the post-BookTok bump sustain?

### B. Segmentation — who tracks books, and why? *(review mining: what job do reviewers describe?)*

- [ ] B1. What distinct user types appear? (Hypotheses to confirm/kill: streak-builders, stats nerds, Goodreads refugees, shelf catalogers, book-club socializers.)
- [ ] B2. Which segment writes the most passionate reviews (5★ *and* 1★)? Passion = retention + word of mouth.
- [ ] B3. Do reviewers mention switching *from* another app? From which, and why? (Migration paths reveal the real competitive graph.)

### C. Unmet needs & pain *(1–3★ review mining per competitor)*

- [ ] C1. **Bookly & Basmo**: which locked *feature* triggers the paywall anger and makes people quit? (That feature = the free tier.)
- [ ] C2. **StoryGraph**: what do mobile users complain about — speed, offline, UX? Is "the app is slow" frequent or just loud?
- [ ] C3. **Goodreads**: what keeps people there despite hating it? (The lock-in to break — likely history/social/reviews.)
- [ ] C4. What do users of simple trackers (Reading List, Bookmory) ask for that the app refuses to add? (Feature ceiling of the minimal segment.)
- [ ] C5. Recurring complaints across **all** competitors? (Category-wide unmet need = strongest wedge. Priors: book-search/metadata quality, forced accounts, sync failures.)

### D. Willingness to pay *(pricing pages + review sentiment about price)*

- [ ] D1. Monetization model per competitor (subscription / one-time / freemium split) and price points.
- [ ] D2. How often do reviews say "I'd happily pay once but won't subscribe"? (The classic indie opening — quantify it.)
- [ ] D3. Do 5★ reviews mention price positively ("worth every penny")? For which apps/features?

### E. Feature benchmarking matrix *(install/screenshot top 5–6 apps)*

- [ ] E1. Which features are **table stakes** (shelves, goal, title search) vs **differentiators** (streaks, session timer, barcode scan, CSV import, offline, no-account)?
- [ ] E2. Taps from app-open to "log today's reading"? (Benchmark core-loop speed; 4+ taps at incumbents makes "fastest logger" a measurable claim.)
- [ ] E3. Who supports Goodreads CSV import, and how well? (Check reviews for "import failed".)
- [ ] E4. Which apps work fully offline / without an account? (Verify by airplane-moding them; check reviews for sync-loss horror stories.)

### Feature matrix skeleton

| Feature | Goodreads | StoryGraph | Bookly | Basmo | Bookmory | Reading List | Fable/Hardcover |
| ------- | --------- | ---------- | ------ | ----- | -------- | ------------ | --------------- |
| Shelves (TBR/Reading/Done) | | | | | | | |
| Yearly goal | | | | | | | |
| Streaks | | | | | | | |
| Session timer | | | | | | | |
| Barcode/ISBN scan | | | | | | | |
| Goodreads CSV import | | | | | | | |
| Works offline | | | | | | | |
| No account required | | | | | | | |
| Stats/charts depth | | | | | | | |
| Taps to log a session | | | | | | | |
| Price model / point | | | | | | | |
| Downloads / rating count | | | | | | | |

## Kill criteria (decided before researching, so the research can say "no")

- If **no** recent app has gained traction → market may be sealed by defaults → reconsider.
- If paywall complaints are rare/mild → the "fair pricing" wedge is weaker than assumed.
- If every pain point is already solved by *some* app with healthy downloads → need a genuinely new wedge, not "same but better".

## Non-goals

- No primary research (interviews/surveys) in this pass.
- No implementation planning until the wedge decision is made from findings.

## Acceptance criteria

- [ ] All A–E questions answered with evidence (links/screenshots/review quotes).
- [ ] Feature matrix filled for the benchmark set.
- [ ] Kill criteria evaluated with an explicit go / no-go / pivot call.
- [ ] Wedge decision recorded in [[book-tracker-competitive-landscape]] (habit-first vs Goodreads-exit vs stats vs TBR).

## Affected repos / files

- None yet — research phase; findings land in this project's `context/`.

## Open questions

- Which wedge does the evidence support? (Deliberately left open pending findings.)

> [!note] Tracking
> Status/progress is tracked in ClickUp, NOT in this note.
