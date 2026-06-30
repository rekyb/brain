---
title: Design Tokens
type: knowledge
project: global
aliases: [Design Token, Tokens, UI Tokens]
tags: [ux, design-systems, design-tokens, reference]
created: 2026-06-27
status: evergreen
source: "Design Tokens W3C Community Group + makandra + Figma"
---

# 🎟️ Design Tokens

> [!abstract] TL;DR
> A **design token** is the smallest *named design decision* — a key/value pair (e.g. `color.primary.500 = #2D6CDF`) defined **once** and reused everywhere. In [[Atomic Design]] terms they're **"sub-atomic"**: atoms (a button) are built *from* tokens (its color, padding, radius). Change the token once → everything updates.

Part of the [[Design Systems]] cluster. The foundation layer of any [[Component Library]].

## Why tokens exist

Hardcoding `#2D6CDF` in 200 places means 200 edits to rebrand. A token names the decision so it lives in one place. Tokens also let you **re-theme** (light/dark, brand A/B) without touching components — see [[Theming]].

## The three layers (alias model)

Mature systems layer tokens so meaning is separated from raw values:

```jsonc
// 1. PRIMITIVE / GLOBAL — raw values, no meaning
"blue.500":  "#2D6CDF",
"gray.900":  "#111827",
"space.4":   "16px",

// 2. SEMANTIC / ALIAS — meaning, references primitives
"color.action":       "{blue.500}",
"color.text.default": "{gray.900}",
"spacing.md":         "{space.4}",

// 3. COMPONENT — scoped to a component, references semantic
"button.bg":      "{color.action}",
"button.padding": "{spacing.md}"
```

Rule of thumb: **components consume semantic/component tokens, never primitives directly.** That's what makes theming a one-line swap.

## What gets tokenized

- **Color** — palette, semantic roles (action, danger, surface, text)
- **Typography** — font family, size, weight, line-height
- **Spacing** — a scale (4, 8, 12, 16…) not arbitrary numbers
- **Sizing / radius / borders**
- **Elevation / shadows**
- **Motion** — durations, easing curves
- **Breakpoints / z-index**

## In code

Tokens are tool-agnostic JSON, then transformed per platform (see [[Design Token Pipelines]]):

```css
/* generated CSS custom properties */
:root {
  --color-action: #2D6CDF;
  --spacing-md: 16px;
  --button-bg: var(--color-action);
}
.button { background: var(--button-bg); padding: var(--spacing-md); }
```

## In Figma

Tokens map to **Variables / styles** — the "sub-atomic" row in the [[Atomic Design in Figma]] mapping. Define them *before* drawing components so atoms inherit them.

> [!tip] Pitfall
> Don't tokenize everything. Token a value when it's a *reused decision*. One-off magic numbers don't need a token — that's just indirection with no payoff.

## Related

- ↑ Consumed by → [[Atoms (atomic design)]] · [[Component Library]]
- See also → [[Theming]] · [[Design Token Pipelines]] · [[Design Systems]] · [[Atomic Design in Figma]]

## Sources

- Design Tokens Community Group — [Format spec (W3C)](https://tr.designtokens.org/format/)
- makandra — [Atomic Design & Design Tokens](https://makandra.de/en/articles/atomic-design-and-design-tokens-665)
- Figma — [Variables & tokens](https://www.figma.com/best-practices/)
