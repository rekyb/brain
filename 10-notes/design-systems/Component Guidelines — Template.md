---
title: Component Guidelines — Template
aliases: [Component Guidelines Template, Guideline Template, DS Doc Template]
tags: [ux, design-systems, guidelines, template, reference]
created: 2026-06-28
status: evergreen
source: "Synthesis of Shopify Polaris, IBM Carbon, Atlassian, Material, Salesforce Lightning"
---

# 📋 Component Guidelines — Template

> [!info] How to use this template
> Copy everything **below the line** into a new note named after the component (e.g. `Button — Guidelines`). Fill every `{{placeholder}}`, delete sections that genuinely don't apply, and link it from your [[Component Library]]. This is the **Guidelines** layer from [[Design Systems]] — usage rules, do/don't, accessibility, and content/voice. Mirrors the structure used by Polaris, Carbon, Atlassian, Material, and Lightning.

This note documents the template and the reasoning. The copy-paste version starts after the divider.

## Why these sections (industry convention)

| Section | Question it answers | Seen in |
|---|---|---|
| Overview | What is it, in one line? | All |
| Anatomy | What parts make it up? | Carbon, Material, Polaris |
| When to use / not | Is this the right component? | Polaris, Atlassian |
| Variants & options | Which version do I pick? | All |
| States & behavior | How does it react? | Carbon, Material |
| Do / Don't | Concrete right vs. wrong | Polaris, Material |
| Content & voice | What words go in it? | Polaris (gold standard), Atlassian |
| Accessibility | Can everyone use it? | All |
| References | Where's the Figma/code? | All |

---

# {{Component Name}}

> [!abstract] Overview
> {{One–two sentences: what this component is and the single job it does. e.g. "Buttons trigger an action or event — a single, immediate user intent."}}

| | |
|---|---|
| **Status** | {{Stable / Beta / Deprecated}} |
| **Since** | {{v1.4}} |
| **Owner** | {{team / person}} |
| **Atomic level** | {{Atom / Molecule / Organism}} → [[Atomic Design]] |
| **Figma** | {{link}} |
| **Code / Storybook** | {{link}} |

## Anatomy

{{Numbered list (or an annotated image) of the parts, mapped to the atoms/tokens they use.}}

1. **{{Container}}** — uses `{{token: radius, color.surface}}`
2. **{{Label}}** — uses `{{token: font, color.text}}`
3. **{{Icon (optional)}}** — uses `{{token: color.icon}}`

> Built from [[Design Tokens]] + nested instances — never hardcoded values.

## When to use

- {{Scenario where this is the right choice}}
- {{Another valid scenario}}

## When **not** to use

- {{Wrong scenario}} → use **[[{{Alternative component}}]]** instead.
- {{Another anti-scenario}} → see [[Patterns]].

## Variants & options

| Variant / prop | Values | Use when |
|---|---|---|
| `{{intent}}` | {{primary / secondary / danger}} | {{guidance}} |
| `{{size}}` | {{sm / md / lg}} | {{guidance}} |
| `{{icon}}` | {{leading / trailing / none}} | {{guidance}} |

> [!tip] One primary per view
> {{Guidance on hierarchy — e.g. "Use only one primary {{component}} per screen; demote the rest to secondary."}}

## States & behavior

Cover every state (your [[Component Library]] standard):

- **Default / Hover / Focus / Active**
- **Disabled** — {{when, and why; explain instead of leaving users guessing}}
- **Loading** — {{spinner / skeleton; preserve layout}}
- **Error** — {{how it's shown}}
- **Empty / Zero** — {{if applicable}}

## Do / Don't

> [!success] Do
> - ✅ {{Concrete right practice}}
> - ✅ {{Another}}

> [!failure] Don't
> - 🚫 {{Concrete wrong practice — ideally the mirror of a Do above}}
> - 🚫 {{Another}}

*(Pair each Don't with the Do it contradicts — show, don't just tell.)*

## Content & voice

- **Label style:** {{Sentence case / Title case}}; {{verb-first, e.g. "Save changes" not "Submit"}}
- **Length:** {{e.g. 1–3 words; never wrap to two lines}}
- **Tone:** {{clear, human, consistent — match your voice & tone guide}}
- **Avoid:** {{jargon, vague labels like "OK"/"Click here", ALL CAPS for meaning}}
- **Microcopy / errors:** {{say *what happened* and *how to fix it*}}

> [!example]
> ✅ "Delete project"  🚫 "Are you sure?" *(as the button label)*

## Accessibility

See [[Accessibility Essentials]] for the full checklist. Component-specific:

- [ ] **Keyboard:** {{Tab to reach, Enter/Space to activate, Esc to dismiss}}
- [ ] **Focus:** visible focus indicator; logical order
- [ ] **Semantics:** uses `{{<button> / role=…}}`; ARIA only to fill gaps
- [ ] **Labels:** {{icon-only needs an accessible name}}
- [ ] **Contrast:** text & UI ≥ WCAG AA (per [[Theming|theme]])
- [ ] **Target size:** ≥ 44×44px

## Related

- [[{{Related component}}]] · [[Patterns]] · [[Component Library]] · [[Design Tokens]]
- [[UI-UX Best Practices]] · [[Accessibility Essentials]]

## Changelog

| Version | Change |
|---|---|
| {{v1.4}} | {{what changed}} |
