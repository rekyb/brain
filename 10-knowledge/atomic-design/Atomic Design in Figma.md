---
title: Atomic Design in Figma
type: knowledge
project: global
aliases: [Atomic Design Figma, Figma Component Library, Atomic Figma]
tags: [ux, design-systems, atomic-design, figma, tooling, reference]
created: 2026-06-27
status: evergreen
source: "Figma Best Practices + Figma Blog"
---

# 🎨 Atomic Design in Figma

> [!abstract] TL;DR
> Figma maps cleanly onto [[Atomic Design]]: **[[Design Tokens]] = atomic decisions**, **components = atoms/molecules/organisms via nesting**, and **component properties/variants** keep the library lean. Build the library as a *separate file* and never detach instances.

Applied companion to the [[Atomic Design]] cluster. See also → [[Atomic Design with AI Agents and MCP]].

## The mapping

| Atomic stage | Figma construct |
| --- | --- |
| Tokens (sub-atomic) | **Variables / styles** — colors, type, spacing, radius |
| [[Atoms (atomic design)\|Atoms]] | Base components: button, input, icon, avatar |
| [[Molecules (atomic design)\|Molecules]] | Components with **nested instances** of atoms |
| [[Organisms (atomic design)\|Organisms]] | Components nesting molecules (header, card grid) |
| [[Templates (atomic design)\|Templates]] | Frames arranging organisms; layout, auto-layout |
| [[Pages (atomic design)\|Pages]] | Frames with real content / populated instances |

## Best practices (from Figma)

- **Tokens first, always.** Define [[Design Tokens]] (variables) before drawing components — they're the shared atomic decisions everything inherits.
- **Nest, don't duplicate.** Build atom components and *nest instances* inside molecules/organisms. Changing the atom updates everything downstream — the whole point of a system.
- **Prefer component properties over variant explosion.** Boolean, text, and instance-swap properties (plus *expose nested instances*) drastically cut the number of variants you must maintain.
- **Use `/` naming for hierarchy.** `Button / Primary / Default` auto-groups in the Assets panel — a lightweight atomic taxonomy.
- **Separate the library file from product files.** The system file *is* the library; product files *consume* it. Mixing them blurs "system component" vs. "one-off."
- **Never detach.** Detaching breaks the link an atomic system depends on.

## When to use / pitfalls

- Great for scaling consistency across a team; overkill for a one-screen prototype.
- Variant sprawl is the #1 Figma anti-pattern — reach for properties first.
- Keep the molecule/organism split pragmatic; Figma doesn't enforce it, so don't over-fold structure that fights reuse.

## Related

- [[Atomic Design]] · [[Design Tokens]] · [[Component Library]] · [[Design Systems]]
- [[Atomic Design with AI Agents and MCP]]

## Sources

- Figma — [Components, styles & shared library best practices](https://www.figma.com/best-practices/components-styles-and-shared-libraries/)
- Figma Blog — [Creating atomic components for an atomic design system in Figma](https://www.figma.com/blog/creating-atomic-components-in-figma/)
- makandra — [Atomic Design & Design Tokens](https://makandra.de/en/articles/atomic-design-and-design-tokens-665)
