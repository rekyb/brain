---
title: Atoms (atomic design)
type: knowledge
project: global
aliases: [UI Atoms, Atomic Design Atoms]
tags: [ux, design-systems, atomic-design, reference]
created: 2026-06-27
status: evergreen
source: "Brad Frost, *Atomic Design* Ch.2 (atomicdesign.bradfrost.com/chapter-2)"
---

# ⚛️ Atoms

> [!abstract] TL;DR
> Atoms are **the foundational building blocks that comprise all our user interfaces** — the smallest functional units. They can't be broken down further without ceasing to be useful.

Part of the [[Atomic Design]] cluster. Next stage up: [[Molecules (atomic design)]].

## What they are

In chemistry, atoms are the basic units of matter, each with unique properties. In UI, *"each interface atom has its own unique properties, such as the dimensions of a hero image, or the font size of a primary heading."* An atom on its own is fairly abstract — a lone label or a color swatch isn't very useful — but it defines the foundational styles everything else inherits.

## Concrete examples

- **HTML element atoms:** form `label`, `input`, `button`
- **Abstract / token-like atoms:** color palette, fonts, animations, base typography
- Anything that is a single, indivisible interface primitive

> These abstract atoms map directly onto [[Design Tokens]] — the "atomic decisions" a whole system is built from.

## When to use / how to think about them

- Define atoms (and tokens) **first** — they're the vocabulary everything else speaks.
- Catalogue atoms in a style guide so the team sees all base styles in one place.
- Don't try to design *meaning* at this level; atoms get their purpose once combined into [[Molecules (atomic design)]].

## Related

- ↑ Combines into → [[Molecules (atomic design)]]
- See also → [[Design Tokens]] · [[Atomic Design]]

## Sources

- Brad Frost — *Atomic Design*, [Ch. 2](https://atomicdesign.bradfrost.com/chapter-2/)
