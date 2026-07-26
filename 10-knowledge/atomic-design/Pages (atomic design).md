---
title: Pages (atomic design)
type: knowledge
project: global
aliases: [UI Pages, Atomic Design Pages]
tags: [ux, design-systems, atomic-design, reference]
created: 2026-06-27
status: evergreen
source: "Brad Frost, *Atomic Design* Ch.2 (atomicdesign.bradfrost.com/chapter-2)"
---

# 📄 Pages

> [!abstract] TL;DR
> Pages are **specific instances of [templates](Templates%20%28atomic%20design%29.md) that show what a UI looks like with real representative content in place** — the highest-fidelity stage, and where the design system gets stress-tested.

Part of the [Atomic Design](Atomic%20Design.md) cluster. One down: [Templates (atomic design)](Templates%20%28atomic%20design%29.md) · back to hub: [Atomic Design](Atomic%20Design.md).

## What they are

A page is a template brought to life with **real content** — actual headlines, copy, images, and data. This is what users ultimately see, and it's where you find out whether the system actually works.

## Why this stage matters most for QA

Pages are *"essential for testing the effectiveness of the underlying design system."* Real content breaks assumptions: a headline that's 3× longer than the placeholder, a user with no avatar, 0 search results, 10,000 results. Observing components under real conditions feeds fixes **back down** the chain — you might adjust a [molecule](Molecules%20%28atomic%20design%29.md) or an [atom](Atoms%20%28atomic%20design%29.md) because a page revealed a flaw.

> [!tip] This closes the abstract ⇄ concrete loop
> Pages are the "concrete" end of atomic design. Bouncing between pages (concrete) and atoms (abstract) is the core feedback loop that makes the methodology valuable — see [Atomic Design](Atomic%20Design.md).

## Concrete example

The **live homepage** with its real text, images, and media — the final user experience, built entirely from the atoms/molecules/organisms defined earlier.

## Related

- ↓ Instance of → [Templates (atomic design)](Templates%20%28atomic%20design%29.md)
- ↺ Findings flow back to → [Atoms (atomic design)](Atoms%20%28atomic%20design%29.md) · [Molecules (atomic design)](Molecules%20%28atomic%20design%29.md)
- See also → [Atomic Design](Atomic%20Design.md)

## Sources

- Brad Frost — *Atomic Design*, [Ch. 2](https://atomicdesign.bradfrost.com/chapter-2/)
