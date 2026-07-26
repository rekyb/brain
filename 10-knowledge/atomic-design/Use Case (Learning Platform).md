---
title: Use Case(Learning Platform)
type: knowledge
project: global
aliases:
  - Atomic Design Playbook
  - Atomic Design Use Case
  - Lumina Learn Playbook
tags:
  - ux
  - design-systems
  - atomic-design
  - playbook
  - process
  - education
  - reference
created: 2026-06-27
status: evergreen
source: Synthesis of Brad Frost's *Atomic Design* + Figma best practices, applied
---

# 🛠️ Atomic Design — Team Playbook (Learning Platform)

> [!abstract] TL;DR
> A step-by-step playbook for a cross-functional squad adopting [Atomic Design](Atomic%20Design.md) on a real product. We follow a fictional-but-realistic learning platform, **Lumina Learn**, from messy UI to a maintainable [design system](../design-systems/Design%20Systems.md) — through tokens, atoms, molecules, organisms, templates, pages, and into code via [AI/MCP](../agentic-ai/Atomic%20Design%20with%20AI%20Agents%20and%20MCP.md).

Applied companion to the [Atomic Design](Atomic%20Design.md) cluster. Read the [hub](Atomic%20Design.md) first for the theory; this note shows *how a team actually does it*.

---

## The scenario

**Product:** *Lumina Learn* — a learner-facing platform: browse courses, take lessons, answer quizzes, track progress, climb a leaderboard. Web + mobile web.

**The squad (small, cross-functional):**

| Role | Person | Owns |
| --- | --- | --- |
| Design System Owner | 1 senior designer | The library, naming, the [atom](Atoms%20%28atomic%20design%29.md)/[molecule](Molecules%20%28atomic%20design%29.md) taxonomy, sign-off |
| Product Designers | 2 | Contribute components, design [pages](Pages%20%28atomic%20design%29.md), flag gaps |
| Frontend Engineers | 2 | Code components, wire **Code Connect**, keep code ↔ design in sync |
| PM | 1 | Prioritizes, protects time for system work, tracks adoption |

**The trigger (why now):** After a year of shipping fast, Lumina Learn has **four different button styles**, three "card" layouts, and inconsistent spacing. Every new screen re-invents UI; design→eng handoff is slow and lossy. Classic symptom — *designing pages, not a system*. (See the [hub](Atomic%20Design.md): "we're not designing pages, we're designing systems of components.")

> [!tip] When to run this playbook
> Adopt atomic design when (a) UI inconsistency is visible to users, (b) the team is repeating itself, and (c) you expect the product to keep growing. For a one-screen prototype, skip it — it's overhead you don't need yet.

---

## Phase 0 — Audit & buy-in *(≈ 1 week)*

**Goal:** agree there's a problem and a shared vocabulary to fix it.

1. **Interface inventory.** Screenshot every existing screen. Group repeated elements (all the buttons together, all the cards together). The duplication makes the case for you.
2. **Run an atomic vocabulary workshop.** Walk the team through atoms → molecules → organisms → templates → pages so "molecule" means the same thing to design, eng, and PM.
3. **Get the PM to carve out time.** Atomic design is an *investment that compounds* — but the first sprint costs more than it saves. Make that explicit.

**Deliverable:** an inventory board + a one-page "why we're doing this" agreed by the squad.

---

## Phase 1 — Foundations: tokens & atoms *(≈ 1 sprint)*

**Goal:** define the shared decisions everything inherits. **Tokens first, always.**

1. **Define [Design Tokens](../design-systems/Design%20Tokens.md)** as Figma variables: color (primary, success/error for quiz feedback, neutrals), type scale, spacing, radius, elevation. These are the "sub-atomic" decisions.
2. **Build the [atoms](Atoms%20%28atomic%20design%29.md)** as components: `Button`, `Input`, `Label`, `Icon`, `Avatar`, `Badge` (e.g. an XP/points badge), `ProgressBar` (lesson completion).
3. **Catalogue them** in a dedicated library file (see Rituals → naming).

> [!example] Lumina atom in focus — `ProgressBar`
> A single indivisible primitive used across lesson cards, the course header, and the dashboard. Defining it once means every progress indicator in the app moves in lockstep.

**Deliverable:** a published token set + an atoms page in the library file.

---

## Phase 2 — Composition: molecules & organisms *(≈ 1–2 sprints)*

**Goal:** assemble atoms into purposeful units, then into recognizable sections.

1. **[Molecules](Molecules%20%28atomic%20design%29.md)** — small groups with *one job each*:
   - `QuizOption` = `Label` + `Radio` + state styling
   - `SearchCourses` = `Label` + `Input` + `Button`
   - `LessonCardMeta` = `Icon` + duration text + `Badge`
2. **[Organisms](Organisms%20%28atomic%20design%29.md)** — standalone sections nesting molecules:
   - `AppHeader` = logo atom + nav molecule + `Avatar` + XP `Badge`
   - `LessonGrid` = the **same** `LessonCard` molecule repeated
   - `QuizBlock` = question text + repeated `QuizOption` + submit `Button`

> [!warning] The molecule/organism debate
> The squad will argue whether `QuizBlock` is a "big molecule" or an "organism." **Don't burn a meeting on it** — if the label sparks more debate than value, you're being too rigid (see [Atomic Design](Atomic%20Design.md) critique). Pick one, write it in the naming doc, move on.

**Deliverable:** molecules + organisms in the library, all built by *nesting* atom instances (never copy-paste).

---

## Phase 3 — Layout & content: templates & pages *(≈ 1 sprint)*

**Goal:** arrange organisms into layouts, then prove them against real content.

1. **[Templates](Templates%20%28atomic%20design%29.md)** — skeletons with placeholder content:
   - `CourseDashboardTemplate` = `AppHeader` + `LessonGrid` + sidebar progress
   - `LessonTemplate` = `AppHeader` + lesson body + `QuizBlock` + footer nav
2. **[Pages](Pages%20%28atomic%20design%29.md)** — pour in **real** content and stress-test:
   - A learner with a 40-character course title, no avatar, 0% progress.
   - A quiz with 6 long answer options on mobile width.
   - An empty state: "No courses yet."

> [!tip] Pages are your QA
> Real content breaks placeholder assumptions. When the 40-char title overflows the `LessonCard`, the fix flows **back down** — adjust the card molecule or a token, not the page. This abstract⇄concrete loop is the whole point.

**Deliverable:** working templates + at least 3 real pages (incl. edge cases) reviewed by the squad.

---

## Phase 4 — Code + AI/MCP *(ongoing)*

**Goal:** make the design system the source of truth for code, and let AI amplify it.

1. **Mirror the hierarchy in code.** Atoms → molecules → organisms as real React components matching the Figma names.
2. **Wire up Code Connect** so the Figma `Button` maps to the actual `<Button>` component.
3. **Use an MCP-connected agent** (e.g. Claude Code + Figma Dev Mode MCP) to generate screens. Because the system is clean and tokenized, the agent **reuses real components** instead of inventing new ones — reaching ~80–85% completeness fast. Full detail: [Atomic Design with AI Agents and MCP](../agentic-ai/Atomic%20Design%20with%20AI%20Agents%20and%20MCP.md).

> [!note] AI amplifies whatever you give it
> The clean atomic system built in Phases 1–3 is *exactly* what makes Phase 4 pay off. Garbage atoms → garbage generation. The investment compounds again here.

**Deliverable:** a coded component library with Code Connect + one AI-assisted screen shipped.

---

## Rituals & cadence *(keep it alive)*

- **Naming convention:** `Category / Variant / State`, e.g. `Button / Primary / Default`. Figma's `/` auto-groups these in Assets. Write the taxonomy down once.
- **Weekly component review (30 min):** new/changed components reviewed by the Design System Owner before merge. Prevents drift.
- **Definition of Done for a component:**
  - [ ] Built from tokens + nested instances (no detached layers)
  - [ ] All states covered (default, hover, focus, disabled, error, empty)
  - [ ] Responsive behavior checked
  - [ ] Named per convention + added to the library
  - [ ] (If coded) Code Connect mapping exists
- **Contribution model:** anyone can *propose* a component; only the Owner *promotes* it into the shared library. Keeps the system coherent.

---

## A worked thread (one feature, all five levels)

Tracing the **lesson browsing** feature end to end:

| Level | Artifact |
| --- | --- |
| Token | `color/primary`, `space/4`, `radius/md` |
| [Atom](Atoms%20%28atomic%20design%29.md) | `Badge` (XP), `ProgressBar`, `Icon` |
| [Molecule](Molecules%20%28atomic%20design%29.md) | `LessonCard` = thumbnail + title + `LessonCardMeta` + `ProgressBar` |
| [Organism](Organisms%20%28atomic%20design%29.md) | `LessonGrid` = repeated `LessonCard` |
| [Template](Templates%20%28atomic%20design%29.md) | `CourseDashboardTemplate` = header + `LessonGrid` + sidebar |
| [Page](Pages%20%28atomic%20design%29.md) | "My Courses" with a real learner's 7 in-progress courses |

Change `radius/md` once → every `LessonCard` corner updates → every grid → every dashboard. That single edit propagating cleanly *is* the payoff.

---

## Pitfalls to avoid

- **Over-engineering early.** Don't build 200 components before shipping anything. Build atoms/molecules *as features need them*.
- **Bikeshedding the taxonomy.** The molecule/organism line is fuzzy by design — decide and document, don't debate.
- **Detaching instances.** Breaks the propagation that makes the system worth having.
- **Library mixed with product files.** Keep the system file separate from product design files.
- **Treating the metaphor as law.** It strains at the top levels and has no neat home for cross-cutting behavior — pair with [Design Tokens](../design-systems/Design%20Tokens.md) and compound patterns. See [Atomic Design](Atomic%20Design.md) critique.

---

## Related

- [Atomic Design](Atomic%20Design.md) (hub) · [Atomic Design in Figma](Atomic%20Design%20in%20Figma.md) · [Atomic Design with AI Agents and MCP](../agentic-ai/Atomic%20Design%20with%20AI%20Agents%20and%20MCP.md)
- Stages: [Atoms (atomic design)](Atoms%20%28atomic%20design%29.md) · [Molecules (atomic design)](Molecules%20%28atomic%20design%29.md) · [Organisms (atomic design)](Organisms%20%28atomic%20design%29.md) · [Templates (atomic design)](Templates%20%28atomic%20design%29.md) · [Pages (atomic design)](Pages%20%28atomic%20design%29.md)
- [Design Systems](../design-systems/Design%20Systems.md) · [Design Tokens](../design-systems/Design%20Tokens.md) · [Component Library](../design-systems/Component%20Library.md)

## Sources

- Brad Frost — *Atomic Design*, [Ch. 1](https://atomicdesign.bradfrost.com/chapter-1/) & [Ch. 2](https://atomicdesign.bradfrost.com/chapter-2/)
- Figma — [Components, styles & shared library best practices](https://www.figma.com/best-practices/components-styles-and-shared-libraries/)
- Figma Blog — [Design Systems and AI: Why MCP Servers Are The Unlock](https://www.figma.com/blog/design-systems-ai-mcp/)
