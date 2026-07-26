---
title: book-tracker-competitive-landscape
aliases: [reading tracker competitors]
type: context
project: book-tracker
status: draft
created: 2026-07-09
updated: 2026-07-09
tags: [competitive-analysis, indie-app]
source:
---

# Book tracker — competitive landscape

> [!abstract] TL;DR
> Nobody cleanly owns **fast, offline, private, habit-first reading tracking**. Goodreads owns the social default but is ancient; StoryGraph owns stats but is web-first and slow on mobile; Bookly/Basmo own the habit angle but anger users with paywalls. The gap: "Duolingo streak energy, but for your own books, and your data stays on your phone."
>
> ⚠️ Draft from brainstorm (2026-07-09) — claims below are priors to be verified by [book-tracker-validation-research-plan](../specs/book-tracker-validation-research-plan.md), not confirmed findings.

## How the market divides

| App | Owns | Weakness (per their own reviews — to verify) |
| --- | ---- | -------------------------------------------- |
| **Goodreads** | Social + reviews + the default | Ancient UX, Amazon-owned, users actively want out |
| **StoryGraph** | Stats & recommendations | Web-first; mobile app reported slow/clunky |
| **Bookly** | Reading session timers, gamification | Aggressive subscription, cluttered |
| **Basmo** | Habit/goal framing | Hard paywall almost immediately |
| **Fable / Hardcover** | Social clubs, Goodreads refugees | Social = network effects a solo dev can't win |
| **Reading List-type apps** | Simple personal log | No habit/streak dimension, stagnant |

## Candidate wedges (undecided — research should settle this)

1. **Habit/streak-first** — consistency is the product; metadata secondary. Maximum reuse of familiar habit-tracker architecture; competes with Bookly/Basmo on fair pricing.
2. **Private Goodreads exit** — CSV import + offline personal library + privacy. Rides existing "goodreads alternative" search demand; that crowd may expect reviews/social eventually.
3. **Stats-nerd tracker** — beautiful fast native charts vs StoryGraph's slow app; polish is the hardest thing to ship solo.
4. **TBR / physical shelf manager** — barcode-scan owned books, owned-vs-read. BookTok-friendly ("TBR" is a searched concept); least overlap with existing skills.

Wedges are not exclusive — the choice is what the App Store listing and v1 *lead* with.

## Solved problems (no build needed)

- **Book metadata**: Google Books API + Open Library — both free, include cover images.
- **Barcode/ISBN scanning**: `expo-camera` handles it out of the box.
- **Goodreads import**: Goodreads exports CSV; parsing is trivial and a strong acquisition feature.

## Rough v1 sketch (pre-validation, not a spec)

Add book (search / ISBN scan / manual) → shelf status (TBR / Reading / Finished); one-tap session logging (pages or minutes); streak + yearly goal + core stats; reading reminder notification; fully local (SQLite/Drizzle + MMKV), no account. Sync later.

## Why it is this way

Picked from [app-idea-candidates](../../../10-knowledge/product-discovery/app-idea-candidates.md) (#14) because: passionate, vocal audience (BookTok, r/52book); incumbent paywall anger = validated demand; and near-total architecture reuse from a habit-tracker pattern (books ≈ habits, sessions ≈ check-offs, streaks ≈ streaks, same offline mutation queue).

## Related

- [book-tracker](../book-tracker.md) — project MOC
- [book-tracker-validation-research-plan](../specs/book-tracker-validation-research-plan.md) — how these priors get tested

## Sources

- Brainstorm session, 2026-07-09 (secondary research pending)
