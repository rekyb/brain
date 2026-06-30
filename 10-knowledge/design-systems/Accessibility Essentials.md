---
title: Accessibility Essentials
aliases: [Accessibility, a11y, Accessibility Essentials, WCAG Basics]
tags: [ux, ui, accessibility, a11y, guidelines, reference]
created: 2026-06-28
status: evergreen
source: "W3C WCAG 2.2 + WebAIM + MDN"
---

# ♿ Accessibility Essentials

> [!abstract] TL;DR
> **Accessibility (a11y)** means everyone can use your product — including people using keyboards, screen readers, magnification, or with low vision/motor/cognitive differences. It's a core [[UI-UX Best Practices|UI/UX best practice]], not an add-on. Anchor on **WCAG's POUR principles** (Perceivable, Operable, Understandable, Robust) and bake it into your [[Design Tokens|tokens]] and [[Component Library|components]] so it scales for free.

Companion to [[UI-UX Best Practices]]. Resolves the a11y/contrast/states threads referenced across the [[Atomic Design]] cluster.

---

## POUR — the four principles

| Principle | Means | Examples |
|---|---|---|
| **Perceivable** | Users can sense the content | Contrast, alt text, captions, not color-only cues |
| **Operable** | Users can interact | Keyboard access, big targets, no time traps |
| **Understandable** | Users can comprehend | Clear labels, predictable behavior, helpful errors |
| **Robust** | Works with assistive tech | Semantic HTML, valid ARIA |

---

## The essentials (highest impact first)

### 1. Color & contrast
- **Text contrast ≥ 4.5:1** (AA); large text ≥ 3:1. UI components/icons ≥ 3:1.
- **Never rely on color alone** — pair with icon/text (e.g. quiz right/wrong needs ✓/✗, not just green/red).
- Bake passing pairs into **semantic [[Design Tokens|tokens]]** and re-check them per [[Theming|theme]] (dark mode often breaks light-mode ratios).

### 2. Keyboard operability
- **Everything works without a mouse** — Tab/Shift-Tab to move, Enter/Space to activate, Esc to dismiss.
- **Logical focus order** that matches visual order.
- **No keyboard traps** — users can always Tab away.

### 3. Visible focus
- Every interactive element shows a **clear focus ring**. Don't `outline: none` without a replacement.
- Make focus a state in every [[Component Library|component]] (your notes already list `focus` as a required state).

### 4. Semantics & ARIA
- **Use the right element** — a real `<button>`, `<a>`, `<input>`, `<nav>`. Native semantics give behavior + AT support free.
- **ARIA only to fill gaps**, never to replace semantics. *No ARIA is better than bad ARIA.*
- Associate **labels with inputs** (`<label for>`); group related fields (`<fieldset>`).

### 5. Images & media
- **Meaningful alt text** for informative images; empty `alt=""` for decorative ones.
- **Captions/transcripts** for video/audio — relevant for a learning platform's lessons.

### 6. Forms (error recovery)
- Errors are **announced, specific, and tied to the field** — not just red borders.
- Say *what* and *how to fix*; **never clear valid input** on submit failure.

### 7. Motion & timing
- Honor **`prefers-reduced-motion`** — offer a calm fallback.
- Avoid flashing (seizure risk); avoid auto-advancing content users can't pause.

### 8. Touch & sizing
- **Targets ≥ 44×44px** with adequate spacing.
- Support zoom to 200% without breaking layout; don't disable pinch-zoom.

---

## Quick checklist

- [ ] Text contrast ≥ 4.5:1 (verified per theme)
- [ ] Information never conveyed by color alone
- [ ] Fully keyboard-operable, logical focus order, no traps
- [ ] Visible focus indicator on every interactive element
- [ ] Semantic elements first; ARIA only where needed
- [ ] Inputs have associated labels; errors are specific + announced
- [ ] Meaningful alt text; decorative images `alt=""`
- [ ] Captions/transcripts for media
- [ ] `prefers-reduced-motion` respected; no flashing
- [ ] Touch targets ≥ 44px; works at 200% zoom

> [!tip] Make it systemic
> The cheapest a11y is built into the [[Design Systems|design system]]: token contrast pairs, a focus-state on every atom, an accessible `Input`/`Button` once. Then every screen inherits it — the same compounding payoff as [[Atomic Design]].

---

## Related

- Part of → [[UI-UX Best Practices]] · enforced via → [[Design Tokens]] · [[Component Library]]
- See also → [[Theming]] · [[Patterns]] · [[Design Systems]]

## Sources

- W3C — [WCAG 2.2 Quick Reference](https://www.w3.org/WAI/WCAG22/quickref/)
- WebAIM — [Contrast Checker](https://webaim.org/resources/contrastchecker/) · [Articles](https://webaim.org/articles/)
- MDN — [Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility)
