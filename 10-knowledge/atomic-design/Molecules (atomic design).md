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
> Molecules are **relatively simple groups of UI elements functioning together as a unit** — a few [[Atoms (atomic design)|atoms]] bonded into something with a single, clear purpose.

Part of the [[Atomic Design]] cluster. One down: [[Atoms (atomic design)]] · one up: [[Organisms (atomic design)]].

## What they are

This is where atoms start to *do something*. Just as identical atoms form different molecules (water vs. hydrogen peroxide) with distinct properties, combining UI atoms into a deliberate group gives them purpose.

## The canonical example

A **search form**: a `label` atom + an `input` atom + a `button` atom. Individually abstract; together they become a reusable component with a job — *"clicking the button atom now submits the form."*

## When to use / how to think about them

- Aim for **one responsibility per molecule** — this is the "single responsibility principle" for UI. It keeps molecules reusable and testable.
- If a molecule is trying to do several things, it's probably an [[Organisms (atomic design)|organism]].
- Molecules are the sweet spot for a [[Component Library]]: small enough to reuse everywhere, meaningful enough to be worth reusing.

> [!warning] Boundary caution
> The line between *molecule* and *organism* is the most-debated part of atomic design. Don't agonize — if classifying it sparks more debate than value, you're being too rigid. See critique in [[Atomic Design]].

## Related

- ↓ Made of → [[Atoms (atomic design)]]
- ↑ Combines into → [[Organisms (atomic design)]]
- See also → [[Atomic Design]] · [[Component Library]]

## Sources

- Brad Frost — *Atomic Design*, [Ch. 2](https://atomicdesign.bradfrost.com/chapter-2/)
