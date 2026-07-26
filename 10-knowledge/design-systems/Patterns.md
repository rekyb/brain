---
title: Patterns
type: knowledge
project: global
aliases: [Pattern Library, Design Patterns, UX Patterns]
tags: [ux, design-systems, patterns, reference]
created: 2026-06-27
status: evergreen
source: "Nielsen Norman Group + Brad Frost"
---

# 🧱 Patterns

> [!abstract] TL;DR
> A **pattern** is a reusable *solution to a recurring design problem* — "how we do forms", "how we handle empty states", "how we paginate". Where a [Component Library](Component%20Library.md) gives you the *parts*, patterns tell you *how to combine them to solve a known problem*. They're the layer of a [Design System](Design%20Systems.md) that captures **behavior and intent**, not just markup.

Part of the [Design Systems](Design%20Systems.md) cluster. Fills a gap [Atomic Design](../atomic-design/Atomic%20Design.md) doesn't fully address (cross-cutting behavior).

## Component vs. pattern

- **Component** — a reusable *thing* (a `Modal`, an `Input`). Lives in the [Component Library](Component%20Library.md).
- **Pattern** — a reusable *approach* to a problem ("destructive-action confirmation", "progressive disclosure", "inline validation"). It may *use* several components.

> Your [Atomic Design](../atomic-design/Atomic%20Design.md) note flags this directly: the strict atom→page hierarchy has *"no home for cross-cutting behavior."* Patterns are that home.

## Common pattern categories

- **Forms & validation** — labels, inline errors, required-field handling
- **Navigation** — global nav, breadcrumbs, tabs, pagination
- **Feedback** — toasts, loading/skeleton states, empty states, error states
- **Data display** — tables, filtering, sorting, infinite scroll vs. pagination
- **Disclosure** — accordions, tooltips, progressive disclosure
- **Confirmation** — destructive-action guards, undo

## What a documented pattern includes

1. **Problem** — when does this apply?
2. **Solution** — the recommended approach + which components to use
3. **Examples** — good usage, with real content
4. **Anti-patterns** — what *not* to do, and why
5. **Accessibility notes** — keyboard, focus, ARIA expectations

## Pattern library

A **pattern library** is the documented collection of these solutions — often living alongside the [Component Library](Component%20Library.md) in the same docs site (e.g. Storybook + MDX guides). Together they make a [Design System](Design%20Systems.md) *prescriptive*, not just a parts bin.

> [!tip] Why it matters
> Two teams can use the same `Button` and `Modal` and still build inconsistent confirmation flows. Patterns standardize the *behavior*, which is where real inconsistency hides.

## Related

- Uses → [Component Library](Component%20Library.md) · built on → [Design Tokens](Design%20Tokens.md)
- Complements → [Atomic Design](../atomic-design/Atomic%20Design.md) · [Compound Components](Compound%20Components.md) · part of → [Design Systems](Design%20Systems.md)

## Sources

- Nielsen Norman Group — [Design Patterns](https://www.nngroup.com/articles/)
- Brad Frost — *Atomic Design* (pattern libraries)
