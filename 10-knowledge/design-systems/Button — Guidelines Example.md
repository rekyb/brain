---
title: Button — Guidelines
type: knowledge
project: global
aliases: [Button Guidelines, Button Component, Lumina Button]
tags: [ux, design-systems, guidelines, component, atom, reference]
created: 2026-06-28
status: evergreen
source: "Worked example of [[Component Guidelines — Template]] — Lumina Learn"
---

# 🔘 Button

> [!abstract] Overview
> Buttons trigger an immediate action or event — a single, clear user intent like *Start lesson*, *Submit quiz*, or *Save progress*. They are the most-used interactive [[Atoms (atomic design)|atom]] in Lumina Learn; consistency here is what killed the "four different button styles" problem from the [[Use Case (Learning Platform)|playbook]].

| | |
|---|---|
| **Status** | Stable |
| **Since** | v1.0 |
| **Owner** | Design System Owner (senior designer) |
| **Atomic level** | Atom → [[Atomic Design]] |
| **Figma** | `Lumina Library / Atoms / Button` |
| **Code / Storybook** | `@lumina/ui` → `<Button>` · Storybook → *Atoms/Button* |

## Anatomy

1. **Container** — uses `radius/md`, `space/3` (vertical) + `space/4` (horizontal), `color/primary` (filled)
2. **Label** — uses `font/body-strong`, `color/on-primary`; verb-first text
3. **Icon (optional)** — uses `color/on-primary`, sized to the label's line-height; leading or trailing
4. **Loading spinner (optional)** — replaces the icon slot, keeps width stable

> Built from [[Design Tokens]] + nested instances — never hardcoded colors or px.

## When to use

- To trigger an **action**: *Submit quiz*, *Mark lesson complete*, *Save changes*.
- To advance a flow: *Next lesson*, *Start course*.
- When the action stays **on the current context** (no full navigation away).

## When **not** to use

- For **navigation to another page/URL** → use a **[[Link]]** (`<a>`), styled as a link or, if it must look like a button, a link-styled button. Screen readers must hear "link," not "button."
- For **toggling a setting on/off** → use a **Switch** / **Checkbox**.
- For **choosing one of several options** → use a **Radio group** or **Segmented control**.
- For many low-priority actions crammed together → see overflow/menu [[Patterns]].

## Variants & options

| Variant / prop | Values | Use when |
|---|---|---|
| `intent` | `primary` | The single most important action in a view (*Submit quiz*) |
| | `secondary` | Supporting actions (*Cancel*, *Back*) |
| | `danger` | Destructive actions (*Delete course*) |
| `size` | `sm` / `md` / `lg` | `md` default; `sm` in dense tables; `lg` for primary CTAs / mobile |
| `icon` | `leading` / `trailing` / `none` | Leading to reinforce meaning; trailing for directional (*Next →*) |
| `fullWidth` | `true` / `false` | `true` on mobile widths or in narrow cards |

> [!tip] One primary per view
> Use **only one `primary` button per screen or section**. If everything is primary, nothing is. Demote the rest to `secondary`. (Visual hierarchy → [[UI-UX Best Practices]].)

## States & behavior

- **Default / Hover / Focus / Active** — distinct visual feedback within ~100ms of interaction.
- **Disabled** — only when the action is genuinely unavailable; pair with context (e.g. helper text "Complete all questions to submit"). Don't disable silently — users can't tell *why*.
- **Loading** — show an inline spinner, keep the label or swap to it, and **keep the button width fixed** so layout doesn't jump. Block repeat clicks.
- **Error** — the button itself doesn't show errors; surface them on the field/form (see [[Accessibility Essentials]] → forms).

## Do / Don't

> [!success] Do
> - ✅ Use a **verb-first, specific** label: *Start lesson*, *Submit quiz*.
> - ✅ Keep **one `primary`** button per view; make it the visually dominant action.
> - ✅ Show a **loading state** on submit to confirm the system heard the click.
> - ✅ Use `danger` styling for destructive actions and confirm them ([[Patterns]]).

> [!failure] Don't
> - 🚫 Use vague labels like *OK*, *Submit*, or *Click here*.
> - 🚫 Stack multiple `primary` buttons competing for attention.
> - 🚫 Use a button to **navigate** — that's a [[Link]]'s job.
> - 🚫 Delete-without-confirm, or rely on red **color alone** to signal danger (add an icon/label).

## Content & voice

- **Label style:** Sentence case, **verb-first** — *Save changes*, not *Changes* or *OK*.
- **Length:** 1–3 words; never wraps to two lines. If you need a sentence, you need different UI.
- **Tone:** clear and human; describe the *outcome* (*Start free trial*) over the mechanic (*Submit*).
- **Avoid:** jargon, ALL CAPS to convey meaning (caps as a *style* via tokens is fine), and ambiguous confirmations as labels.
- **Microcopy:** the button states the action; errors/explanations live near the relevant field.

> [!example]
> ✅ "Submit quiz"  ✅ "Delete course"  🚫 "OK"  🚫 "Are you sure?" *(as a button label)*

## Accessibility

See [[Accessibility Essentials]] for the full checklist. Button-specific:

- [ ] **Keyboard:** reachable via Tab; activates on **Enter** *and* **Space**.
- [ ] **Focus:** visible focus ring (uses `color/focus`); never `outline: none` without a replacement.
- [ ] **Semantics:** real `<button>` element — *not* a styled `<div>`. Type set (`submit`/`button`).
- [ ] **Icon-only buttons:** must have an accessible name (`aria-label`), e.g. a bookmark icon → "Save course."
- [ ] **Disabled vs. aria-disabled:** prefer `disabled`; if it must stay focusable to explain why, use `aria-disabled` + helper text.
- [ ] **Loading:** announce state (`aria-busy` / live region) so it isn't silent for screen readers.
- [ ] **Contrast:** label vs. fill ≥ 4.5:1; verify in **both** light and [[Theming|dark themes]].
- [ ] **Target size:** ≥ 44×44px (especially `sm` on touch).

## Related

- [[Link]] · [[Patterns]] · [[Component Library]] · [[Design Tokens]] · [[Atoms (atomic design)]]
- [[UI-UX Best Practices]] · [[Accessibility Essentials]] · [[Component Guidelines — Template]]
- Applied context → [[Use Case (Learning Platform)]]

## Changelog

| Version | Change |
|---|---|
| v1.0 | Initial `primary` / `secondary` / `danger` variants; replaced the four legacy button styles. |
| v1.3 | Added `loading` state + fixed-width behavior. |
| v1.4 | Added `fullWidth` for mobile; dark-theme contrast pass. |
