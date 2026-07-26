---
title: Design Systems
type: knowledge
project: global
aliases: [Design System, Design System (DS)]
tags: [ux, design-systems, reference, moc]
created: 2026-06-27
status: evergreen
source: "Brad Frost, *Atomic Design* Ch.5 + Nielsen Norman Group"
---

# 🧰 Design Systems

> [!abstract] TL;DR
> A **design system** is the single source of truth that bundles everything a team needs to design and build a product consistently: **[Design Tokens](Design%20Tokens.md)** (the decisions), a **[Component Library](Component%20Library.md)** (the reusable parts), **[Patterns](Patterns.md)** (reusable solutions), and the **guidelines + docs** that say how to use them. [Atomic Design](../atomic-design/Atomic%20Design.md) is the *mental model* you use to organize it; the design system is the *thing you ship*.

Hub note for the design-system cluster. Pairs with the [Atomic Design](../atomic-design/Atomic%20Design.md) cluster.

## What a design system actually contains

A common mistake is thinking "design system = component library." The library is one layer. The full stack:

| Layer                    | What it is                                                          | Note                  |
| ------------------------ | ------------------------------------------------------------------- | --------------------- |
| **Foundations / tokens** | Color, type, spacing, radius, motion — the atomic decisions         | [Design Tokens](Design%20Tokens.md)     |
| **Components**           | Reusable UI built from tokens (atoms → organisms)                   | [Component Library](Component%20Library.md) |
| **Patterns**             | Reusable solutions to recurring problems (forms, empty states, nav) | [Patterns](Patterns.md)          |
| **Guidelines**           | Usage rules, do/don't, accessibility, content/voice                 | [Component Guidelines — Template](Component%20Guidelines%20%E2%80%94%20Template.md) |
| **Shared vocabulary**    | "Molecule" / "card" means the same to design, eng, PM               | [Atomic Design](../atomic-design/Atomic%20Design.md)     |

## Why teams build one

- **Consistency at scale.** One change to a token or atom propagates everywhere — investment compounds (the core payoff of [Atomic Design](../atomic-design/Atomic%20Design.md)).
- **Speed.** Teams assemble screens from vetted parts instead of redrawing primitives.
- **Shared language.** Designers and developers stop talking past each other.
- **Quality floor.** Accessibility, theming, and states are solved once, centrally.

## Design system vs. style guide vs. component library

- **Style guide** — documentation of *rules* (how things should look/behave). A part of the DS.
- **Component library** — the actual reusable *parts*. A part of the DS. See [Component Library](Component%20Library.md).
- **Design system** — the *whole*: tokens + components + patterns + guidelines + governance.

## Governance (the part people skip)

A design system is a *product*, not a project. It needs:

- An **owner/team** and a contribution model (who can add a component, and how).
- **Versioning** and release notes (it's depended on by other teams).
- A **deprecation path** so old patterns don't linger forever.
- Living docs co-located with the code/Figma library.

> [!tip] Pitfall
> Don't build the whole system up front. Start with tokens + a handful of high-use atoms, ship real screens, and grow the system from real demand. A DS nobody uses is overhead.

## Related

- [Design Tokens](Design%20Tokens.md) · [Component Library](Component%20Library.md) · [Patterns](Patterns.md) · [Theming](Theming.md)
- [Atomic Design](../atomic-design/Atomic%20Design.md) · [Atomic Design in Figma](../atomic-design/Atomic%20Design%20in%20Figma.md) · [Atomic Design with AI Agents and MCP](../agentic-ai/Atomic%20Design%20with%20AI%20Agents%20and%20MCP.md)

## Sources

- Brad Frost — *Atomic Design*, [Ch. 5: Maintaining Atomic Design Systems](https://atomicdesign.bradfrost.com/chapter-5/)
- Nielsen Norman Group — [Design Systems 101](https://www.nngroup.com/articles/design-systems-101/)
