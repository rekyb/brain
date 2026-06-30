---
title: UI-UX Best Practices
type: knowledge
project: global
aliases: [UI/UX Best Practices, UI UX Guidelines, Design Guidelines]
tags: [ux, ui, design-systems, guidelines, reference, moc]
created: 2026-06-28
status: evergreen
source: "Synthesis of vault notes + Nielsen Norman Group + WCAG"
---

# ✅ UI/UX Best Practices

> [!abstract] TL;DR
> Good UI/UX rests on two pillars: **(1) build a system, don't design pages** — the [[Atomic Design]] + [[Design Systems]] discipline already in this vault; and **(2) design for humans** — usability heuristics, visual hierarchy, [[Accessibility Essentials|accessibility]], feedback, and clear content. This note is the practical guideline that ties both together, with a copy-paste checklist at the end.

Hub note. Synthesizes the [[Atomic Design]] and [[Design Systems]] clusters into actionable rules.

---

## Pillar 1 — Build a system (from this vault)

These come straight from your atomic-design notes and are the *structural* half of good UI/UX.

| Practice | Why | Source note |
|---|---|---|
| **Tokens first, always** | Foundational decisions everything inherits; one change propagates | [[Design Tokens]] · [[Atoms (atomic design)]] |
| **Nest, don't duplicate** | Change an atom once → every molecule/organism updates | [[Atomic Design in Figma]] · [[Component Library]] |
| **One responsibility per molecule** | Keeps components reusable and testable | [[Molecules (atomic design)]] |
| **Context-independent organisms** | A header should work on any page | [[Organisms (atomic design)]] |
| **Cover every state** | default · hover · focus · disabled · loading · error · empty | [[Pages (atomic design)]] |
| **Pages are your QA** | Real content breaks placeholder assumptions; fixes flow back down | [[Pages (atomic design)]] |
| **Rigorous naming** | `Category / Variant / State` — the team's *and* AI's vocabulary | [[Atomic Design with AI Agents and MCP]] |
| **Don't over-engineer** | Build components as features need them, not 200 up front | [[Atomic Design]] |

> [!tip] The mental model
> *"We're not designing pages, we're designing systems of components."* Zoom abstract⇄concrete — atom ↔ page — as your core feedback loop. ([[Atomic Design]])

---

## Pillar 2 — Design for humans

The *experience* half — what the system above is in service of.

### Usability heuristics (Nielsen's 10, condensed)

1. **Visibility of system status** — always show what's happening (loading, saved, progress). See feedback below.
2. **Match the real world** — use users' language, not internal jargon.
3. **User control & freedom** — easy undo/cancel; clear exits.
4. **Consistency & standards** — same thing looks/behaves the same way (this is what a [[Design Systems|design system]] enforces).
5. **Error prevention** — disable invalid actions, confirm destructive ones. See [[Patterns]].
6. **Recognition over recall** — show options; don't make users remember.
7. **Flexibility & efficiency** — shortcuts for experts, simple paths for novices.
8. **Aesthetic & minimalist design** — every extra element competes for attention.
9. **Help users recover from errors** — plain-language messages that say *what* and *how to fix*.
10. **Help & documentation** — available when needed, in context.

### Visual hierarchy & layout

- **One primary action per screen.** Make it the most prominent thing; demote the rest.
- **Use a spacing scale**, not arbitrary px — rhythm comes from a token scale ([[Design Tokens]]).
- **Group related things** (proximity); separate unrelated things (Gestalt).
- **Limit type styles.** A small, deliberate type scale beats ad-hoc sizes.
- **Respect alignment & a grid.** Misalignment reads as "broken" even when users can't name why.
- **Whitespace is not wasted space** — it's how you create focus.

### Interaction & feedback

- **Every action gets a reaction** within ~100ms (visual press, spinner, optimistic update).
- **Make affordances obvious** — buttons look clickable; disabled looks disabled.
- **Design the unhappy paths**: loading (skeletons), empty states, errors, zero/overflow content. (Your [[Pages (atomic design)|pages]] note: 0 results vs. 10,000 results.)
- **Preserve user input** — never clear a form on error.
- **Motion with purpose** — guide attention; keep it fast (~150–250ms) and respect reduced-motion.

### Accessibility (non-negotiable)

Bake it in, don't bolt it on. Color contrast, keyboard operability, focus order, semantics, alt text. Full guidance → [[Accessibility Essentials]].

### Content & UX writing

- **Clarity over cleverness.** Labels and buttons say exactly what happens (`Save changes`, not `OK`).
- **Front-load meaning** — users scan, they don't read.
- **Write helpful errors** — "Email already in use — try signing in" beats "Error 422."
- **Be consistent in terminology** — one name per concept, everywhere.

---

## The master checklist

Use per screen / per component before shipping:

**System**
- [ ] Built from [[Design Tokens|tokens]] + nested instances (no detached/hardcoded values)
- [ ] Named per `Category / Variant / State` convention
- [ ] All states present: default · hover · focus · disabled · loading · error · empty
- [ ] Reuses existing [[Component Library|components]] (didn't reinvent)

**Usability**
- [ ] One clear primary action; secondary actions demoted
- [ ] System status is always visible (loading/saved/progress)
- [ ] Destructive actions are confirmed; invalid ones prevented
- [ ] Errors say *what happened* and *how to fix it*; input is preserved

**Visual**
- [ ] Consistent spacing scale, alignment, and type scale
- [ ] Clear hierarchy — the eye knows where to look first
- [ ] Tested with real + edge content (long text, no image, 0 / many items)

**Accessibility** → see [[Accessibility Essentials]]
- [ ] Contrast ≥ WCAG AA (4.5:1 text)
- [ ] Fully keyboard-operable with visible focus
- [ ] Semantic markup / labels; meaningful alt text
- [ ] Honors `prefers-reduced-motion`

**Responsive**
- [ ] Works across breakpoints; touch targets ≥ 44px

---

## Pitfalls (from the vault)

- **Over-engineering early** — ship value, grow the system from real demand. ([[Use Case (Learning Platform)|Playbook]])
- **Bikeshedding the taxonomy** — molecule vs. organism is fuzzy by design; decide, document, move on.
- **Detaching instances** — breaks the propagation that makes a system worth having.
- **Treating the metaphor as law** — pair atomic design with [[Design Tokens]], [[Patterns]], and [[Compound Components]] for cross-cutting behavior.
- **Accessibility as an afterthought** — far cheaper designed-in than retrofitted.

---

## Related

- [[Atomic Design]] · [[Design Systems]] · [[Design Tokens]] · [[Component Library]] · [[Patterns]]
- [[Accessibility Essentials]] · [[Theming]] · [[Compound Components]]
- Applied: [[Use Case (Learning Platform)]] · [[Atomic Design with AI Agents and MCP]]

## Sources

- Nielsen Norman Group — [10 Usability Heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/)
- Brad Frost — *Atomic Design* (atomicdesign.bradfrost.com)
- W3C — [WCAG 2.2 Quick Reference](https://www.w3.org/WAI/WCAG22/quickref/)
- Figma — [Design system best practices](https://www.figma.com/best-practices/)
