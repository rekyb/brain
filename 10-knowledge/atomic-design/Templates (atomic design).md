---
title: Templates (atomic design)
aliases: [UI Templates, Atomic Design Templates]
tags: [ux, design-systems, atomic-design, reference]
created: 2026-06-27
status: evergreen
source: "Brad Frost, *Atomic Design* Ch.2 (atomicdesign.bradfrost.com/chapter-2)"
---

# 📐 Templates

> [!abstract] TL;DR
> Templates are **page-level objects that place components into a layout and articulate the design's underlying content structure** — the skeleton, not the skin.

Part of the [[Atomic Design]] cluster. One down: [[Organisms (atomic design)]] · one up: [[Pages (atomic design)]].

## What they are

A template is where [[Organisms (atomic design)|organisms]] get arranged into an actual page layout. Crucially, templates *"focus on the page's underlying content structure rather than the page's final content."* They use **placeholder content** — lorem ipsum, grey boxes, sample image dimensions, representative character counts — to show how the structure behaves before real content arrives.

This is the stage that leaves the chemistry metaphor behind and uses plain client-facing language ("template" makes sense to stakeholders in a way "organism" doesn't).

## Concrete example

A **homepage template**: the arrangement of header, hero, feature grid, and footer organisms — shown as a wireframe-like skeleton that demonstrates *where things go and how big they are*, without committing to final copy or imagery.

## When to use / how to think about them

- Use templates to **define guardrails**: max headline length, image aspect ratios, how content reflows.
- Test the *structure* here — does the layout hold up with long names, missing images, or empty states?
- A template becomes a [[Pages (atomic design)|page]] the moment you pour real content in.

## Related

- ↓ Arranges → [[Organisms (atomic design)]]
- ↑ Instantiated as → [[Pages (atomic design)]]
- See also → [[Atomic Design]]

## Sources

- Brad Frost — *Atomic Design*, [Ch. 2](https://atomicdesign.bradfrost.com/chapter-2/)
