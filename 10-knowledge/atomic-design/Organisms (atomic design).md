---
title: Organisms (atomic design)
type: knowledge
project: global
aliases: [UI Organisms, Atomic Design Organisms]
tags: [ux, design-systems, atomic-design, reference]
created: 2026-06-27
status: evergreen
source: "Brad Frost, *Atomic Design* Ch.2 (atomicdesign.bradfrost.com/chapter-2)"
---

# 🦠 Organisms

> [!abstract] TL;DR
> Organisms are **relatively complex UI components composed of groups of [[Molecules (atomic design)|molecules]] and/or [[Atoms (atomic design)|atoms]] and/or other organisms** — distinct, recognizable sections of an interface.

Part of the [[Atomic Design]] cluster. One down: [[Molecules (atomic design)]] · one up: [[Templates (atomic design)]].

## What they are

Organisms are where the UI becomes something a user recognizes as a "section." They assemble smaller pieces into standalone chunks of an interface that can be dropped into any layout.

## Concrete examples

- A **website header**: logo atom + navigation molecule + search-form molecule.
- A **product grid**: the *same* product-card molecule repeated many times.

Both patterns are common: organisms can mix *different* molecules, or *repeat the same one*.

## When to use / how to think about them

- Build organisms to be **context-independent** — a header should work on any page.
- They're the right granularity for "give me the whole nav" conversations with stakeholders.
- Organisms feed into [[Templates (atomic design)|templates]], which arrange them on the page.

> [!note] This is the top of the clean metaphor
> Above organisms, the chemistry analogy starts to strain — *templates* and *pages* are layout/content concepts, not "bigger molecules." That's expected; see [[Atomic Design]] critique.

## Related

- ↓ Made of → [[Molecules (atomic design)]] · [[Atoms (atomic design)]]
- ↑ Arranged by → [[Templates (atomic design)]]
- See also → [[Atomic Design]]

## Sources

- Brad Frost — *Atomic Design*, [Ch. 2](https://atomicdesign.bradfrost.com/chapter-2/)
