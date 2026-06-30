---
title: Design Token Pipelines
type: knowledge
project: global
aliases: [Token Pipeline, Style Dictionary, Token Transformation]
tags: [ux, design-systems, design-tokens, tooling, reference]
created: 2026-06-27
status: evergreen
source: "Style Dictionary (Amazon) + Design Tokens W3C CG + Figma"
---

# 🔧 Design Token Pipelines

> [!abstract] TL;DR
> A **token pipeline** turns one source of truth — your [[Design Tokens]] as platform-agnostic JSON — into the many formats each platform needs (CSS variables, JS, iOS, Android, Figma). Tools like **Style Dictionary** read the JSON and *transform + build* outputs, so design and code never drift. This is the plumbing that keeps a [[Design Systems|Design System]] in sync.

Part of the [[Design Systems]] cluster. The operational backbone for [[Design Tokens]] and [[Theming]].

## The problem

Without a pipeline, the same value (`#2D6CDF`) is typed by hand into Figma, CSS, Swift, and Kotlin — and they drift the moment one changes. A pipeline makes **one JSON source** the origin and generates the rest.

## The flow

```
Source tokens (JSON, W3C format)
        │  transform (resolve aliases, change units/format)
        ▼
   Build per platform
   ├── web    → CSS variables / JS / SCSS
   ├── iOS    → Swift
   ├── Android→ XML
   └── Figma  → Variables (via plugin/API)
```

## Example: Style Dictionary

Source:
```json
{ "color": { "action": { "value": "#2D6CDF", "type": "color" } } }
```
Config (which platforms to build):
```json
{
  "source": ["tokens/**/*.json"],
  "platforms": {
    "css": { "transformGroup": "css", "buildPath": "dist/",
      "files": [{ "destination": "tokens.css", "format": "css/variables" }] }
  }
}
```
Generated output:
```css
:root { --color-action: #2D6CDF; }
```

Run the build → every platform gets a fresh, consistent artifact.

## Where Figma fits

Tokens can flow **both directions**:
- **Figma → code** — export Figma Variables to JSON, feed the pipeline (designers as source of truth).
- **Code → Figma** — push JSON into Figma Variables (engineering as source of truth).

Either way, pick *one* source of truth; see [[Atomic Design in Figma]] for the design side.

## Good practice

- **One source of truth.** Decide whether Figma or the repo owns tokens — not both.
- **Automate the build in CI.** Regenerate artifacts on token change; commit nothing by hand.
- **Use the W3C token format.** Future-proof and tool-interoperable (see [[Design Tokens]]).
- **Version the output.** Token releases are breaking changes for consumers.

> [!tip] Payoff
> This is what makes [[Theming]] and rebrands cheap: change the source once, run the build, every platform updates together.

## Related

- Transforms → [[Design Tokens]] · enables → [[Theming]] · feeds → [[Component Library]]
- Bridges → [[Atomic Design in Figma]] · part of → [[Design Systems]]

## Sources

- Amazon — [Style Dictionary](https://amzn.github.io/style-dictionary/)
- Design Tokens Community Group — [Format spec (W3C)](https://tr.designtokens.org/format/)
- Figma — [Variables REST API](https://www.figma.com/developers/api#variables)
