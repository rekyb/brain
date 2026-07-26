---
title: Molecules (atomic design)
type: knowledge
project: global
aliases: [UI Molecules, Atomic Design Molecules]
tags: [ux, design-systems, atomic-design, reference]
created: 2026-06-27
status: evergreen
source: "Brad Frost, *Atomic Design* Ch.2 (atomicdesign.bradfrost.com/chapter-2)"
---

# 🧫 Molecules

> [!abstract] TL;DR
> Molecules are **relatively simple groups of UI elements functioning together as a unit** — a few [atoms](Atoms%20%28atomic%20design%29.md) bonded into something with a single, clear purpose.

Part of the [Atomic Design](Atomic%20Design.md) cluster. One down: [Atoms (atomic design)](Atoms%20%28atomic%20design%29.md) · one up: [Organisms (atomic design)](Organisms%20%28atomic%20design%29.md).

## What they are

This is where atoms start to *do something*. Just as identical atoms form different molecules (water vs. hydrogen peroxide) with distinct properties, combining UI atoms into a deliberate group gives them purpose.

## The canonical example

A **search form**: a `label` atom + an `input` atom + a `button` atom. Individually abstract; together they become a reusable component with a job — *"clicking the button atom now submits the form."*

## When to use / how to think about them

- Aim for **one responsibility per molecule** — this is the "single responsibility principle" for UI. It keeps molecules reusable and testable.
- If a molecule is trying to do several things, it's probably an [organism](Organisms%20%28atomic%20design%29.md).
- Molecules are the sweet spot for a [Component Library](../design-systems/Component%20Library.md): small enough to reuse everywhere, meaningful enough to be worth reusing.

> [!warning] Boundary caution
> The line between *molecule* and *organism* is the most-debated part of atomic design. Don't agonize — if classifying it sparks more debate than value, you're being too rigid. See critique in [Atomic Design](Atomic%20Design.md).

## Related

- ↓ Made of → [Atoms (atomic design)](Atoms%20%28atomic%20design%29.md)
- ↑ Combines into → [Organisms (atomic design)](Organisms%20%28atomic%20design%29.md)
- See also → [Atomic Design](Atomic%20Design.md) · [Component Library](../design-systems/Component%20Library.md)

## Sources

- Brad Frost — *Atomic Design*, [Ch. 2](https://atomicdesign.bradfrost.com/chapter-2/)
