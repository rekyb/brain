---
title: Theming
type: knowledge
project: global
aliases: [Theming, Themes, Dark Mode, Multi-brand]
tags: [ux, design-systems, design-tokens, theming, reference]
created: 2026-06-27
status: evergreen
source: "Design Tokens W3C CG + CSS custom properties"
---

# 🌗 Theming

> [!abstract] TL;DR
> **Theming** is swapping the *values* behind your [[Design Tokens]] without changing components — light/dark mode, multi-brand, high-contrast. It works because components reference **semantic tokens** (`color.surface`), and a theme just remaps those tokens to different **primitive** values. One swap, whole UI re-skins.

Part of the [[Design Systems]] cluster. The payoff of the layered token model in [[Design Tokens]].

## Why semantic tokens make it possible

If a button hardcodes `#fff`, you can't theme it. If it uses `color.surface`, you re-point that semantic token per theme:

```jsonc
// light theme
"color.surface": "{white}",
"color.text":    "{gray.900}",

// dark theme
"color.surface": "{gray.900}",
"color.text":    "{gray.50}"
```

Components never change — only the mapping does. (See the 3-layer model in [[Design Tokens]].)

## Implementation: CSS custom properties

The most common runtime mechanism. Define theme values on a scope, components read variables:

```css
:root              { --color-surface: #ffffff; --color-text: #111827; }
[data-theme="dark"]{ --color-surface: #111827; --color-text: #f9fafb; }

.card { background: var(--color-surface); color: var(--color-text); }
```
```js
// toggle by flipping one attribute
document.documentElement.dataset.theme = "dark";
```

## Kinds of theming

- **Mode** — light / dark / high-contrast (accessibility).
- **Brand / white-label** — same components, different brand palettes & type.
- **Density** — comfortable vs. compact (spacing tokens).
- **Per-surface** — a section that inverts (e.g. a dark hero on a light page).

## Good practice

- **Theme via semantic tokens only.** Never theme primitives or component internals directly.
- **Respect user preference.** Honor `prefers-color-scheme` as the default, allow override.
- **Test contrast per theme.** Dark mode often breaks WCAG ratios that passed in light.
- **Avoid theme-specific component code.** If a component needs `if (dark)` logic, a token is missing.

> [!tip] Pitfall
> "Just darken everything" produces muddy dark modes. Dark themes need their own tuned palette (elevation via lighter surfaces, reduced pure-white text), not an inverted light theme.

## Related

- Powered by → [[Design Tokens]] · applied across → [[Component Library]]
- Part of → [[Design Systems]] · piped via → [[Design Token Pipelines]]

## Sources

- MDN — [Using CSS custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)
- Design Tokens Community Group — [Format spec](https://tr.designtokens.org/format/)
