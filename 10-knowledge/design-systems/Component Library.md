---
title: Component Library
type: knowledge
project: global
aliases: [Component Library, UI Library, Component Kit]
tags: [ux, design-systems, components, reference]
created: 2026-06-27
status: evergreen
source: "Brad Frost, *Atomic Design* + Storybook docs"
---

# 🧩 Component Library

> [!abstract] TL;DR
> A **component library** is the concrete, reusable collection of UI parts — buttons, inputs, cards, headers — built from [Design Tokens](Design%20Tokens.md) and organized with [Atomic Design](../atomic-design/Atomic%20Design.md) (atoms → molecules → organisms). It's the *implementation* layer of a [Design System](Design%20Systems.md): the part you actually import into product screens.

Part of the [Design Systems](Design%20Systems.md) cluster. The buildable counterpart to the [Atomic Design](../atomic-design/Atomic%20Design.md) stages.

## Library vs. design system

- **Design system** = tokens + components + [Patterns](Patterns.md) + guidelines + governance (the whole).
- **Component library** = just the reusable *parts* (one layer of the whole).

A library can exist in two places that must stay in sync:

| Surface | What it is |
|---|---|
| **Design library** | The Figma/Sketch file of components — see [Atomic Design in Figma](../atomic-design/Atomic%20Design%20in%20Figma.md) |
| **Code library** | The package (React/Vue/Web Components) product teams `npm install` |

## How it's organized (atomic)

```
tokens/         → color, spacing, type   (sub-atomic, see [Design Tokens](Design%20Tokens.md))
atoms/          → Button, Input, Icon, Avatar
molecules/      → SearchBar, FormField, Card
organisms/      → Header, ProductGrid, Footer
```

Each component should ship with: **all states** (default/hover/focus/disabled/loading/error), **variants** (size, intent), accessibility baked in, and docs/examples.

## What makes a library good

- **Composability** — small parts combine into bigger ones (nest, don't duplicate).
- **Token-driven** — no hardcoded values; restyles flow from [Design Tokens](Design%20Tokens.md).
- **A documented public API** — clear, stable props; see [Compound Components](Compound%20Components.md) for flexible APIs.
- **Discoverable docs** — a living catalog like **Storybook** showing every component + state.
- **Versioned** — semver + changelog, because product teams depend on it.

## Example: a token-driven atom

```jsx
// Button atom — styling comes entirely from tokens
function Button({ intent = "primary", children }) {
  return <button className={`btn btn--${intent}`}>{children}</button>;
}
```
```css
.btn--primary { background: var(--button-bg); padding: var(--spacing-md); }
```

> [!tip] Pitfall
> A library is only valuable if teams *use it instead of rebuilding*. Low adoption usually means missing components, poor docs, or a painful contribution process — fix those before adding more parts.

## Related

- Built from → [Design Tokens](Design%20Tokens.md) · organized by → [Atomic Design](../atomic-design/Atomic%20Design.md)
- See also → [Compound Components](Compound%20Components.md) · [Design Systems](Design%20Systems.md) · [Atomic Design in Figma](../atomic-design/Atomic%20Design%20in%20Figma.md) · [Patterns](Patterns.md)

## Sources

- Brad Frost — *Atomic Design*, [Ch. 4 & 5](https://atomicdesign.bradfrost.com/)
- Storybook — [Component-driven development](https://storybook.js.org/docs)
