# Vault Reorganization for Context- & Spec-Driven Agentic AI Development

- **Date:** 2026-06-30
- **Status:** Approved (design)
- **Author:** rekyb (with Claude Code)

## Goal

Reorganize the `brain` Obsidian vault so it serves two jobs at once:

1. A **reusable, project-agnostic knowledge base** (design systems, atomic design, agentic-AI conventions).
2. An **authoritative home for per-project context and specs**, consumed by AI agents as broad background context while they work in separate code repos.

The vault must keep working natively in Obsidian (wikilinks, graph, properties) while becoming reliably navigable by AI agents pointed at the folder.

## Background & current state

Today the vault is a clean human-browsing "second brain":

- `10-notes/design-systems/` — 11 evergreen notes (tokens, component library, theming, a11y, patterns).
- `10-notes/atomic-design/` — 9 notes (atoms→pages, plus "Atomic Design with AI Agents and MCP").
- `10-notes/edbot-v2/` — 5 notes documenting the real Edbot v2 project (4-repo learning/employability platform, removable-module architecture, `local-main` integration tooling, ClickUp-tracked epics).
- `README.md` proposing a PARA-style layout that is mostly aspirational (only `10-notes/` exists).

**Lesson to preserve:** the Edbot notes record that markdown *trackers* (`STATUS.md`, roadmap status) were deliberately moved out of markdown into ClickUp + in-repo `docs/prd/` because they caused cross-machine merge conflicts. The reorg must keep **specs** (durable "what to build") in the vault while keeping **status/tracking** out of it (ClickUp remains source of truth for tracking).

## Decisions (from brainstorming)

- **Consumption model:** agents read the vault broadly as background context (not a one-spec-at-a-time handoff).
- **Scope:** a reusable knowledge base + per-project areas; Edbot is the first project, structure must hold more.
- **Spec ownership:** the vault is the authoritative home for **both** context and specs, with a **visible separation** between the two.
- **Rules:** add a **root `CLAUDE.md`** only (no per-project CLAUDE.md for now).
- **Approach chosen:** Project-first with type sub-folders inside each project (Approach A), over global type-first (B) or flat/tag-only (C).

## Target structure

```
brain/
├── CLAUDE.md              # NEW — how agents read & use this vault (the rules)
├── README.md              # updated for the new layout
├── 00-inbox/              # quick capture, unprocessed
├── 10-knowledge/          # reusable, project-AGNOSTIC evergreen notes
│   ├── design-systems/    # moved from 10-notes/design-systems
│   ├── atomic-design/     # moved from 10-notes/atomic-design
│   └── agentic-ai/        # NEW — own conventions: spec-driven dev, context
│                          #   engineering, MCP, CLAUDE.md patterns.
│                          #   "Atomic Design with AI Agents and MCP" moves here.
├── 20-projects/
│   └── edbot-v2/
│       ├── edbot-v2.md    # project MOC/index (existing hub note)
│       ├── context/       # durable understanding agents read to orient
│       │                  #   (the 4 existing spoke notes)
│       └── specs/         # authoritative "what to build" specs/PRDs
├── 40-references/         # external sources, clippings, quotes
├── 90-templates/          # NEW — note / context / spec / project templates
└── attachments/           # binaries (created on first use)
```

### File moves (explicit)

| From | To |
| ---- | -- |
| `10-notes/design-systems/*` | `10-knowledge/design-systems/*` |
| `10-notes/atomic-design/Atomic Design with AI Agents and MCP.md` | `10-knowledge/agentic-ai/` |
| `10-notes/atomic-design/*` (remaining) | `10-knowledge/atomic-design/*` |
| `10-notes/edbot-v2/Edbot v2.md` | `20-projects/edbot-v2/edbot-v2.md` (MOC) |
| `10-notes/edbot-v2/Edbot Workspace (Meta-Repo).md` | `20-projects/edbot-v2/context/` |
| `10-notes/edbot-v2/Module Contract (Core-Module-Integration).md` | `20-projects/edbot-v2/context/` |
| `10-notes/edbot-v2/Edbot Roadmap & Epics.md` | `20-projects/edbot-v2/context/` |
| `10-notes/edbot-v2/LEDGER Admin Prototype.md` | `20-projects/edbot-v2/context/` |

Empty `10-notes/` is removed after moves. Moves use `git mv` so history is preserved. Wikilinks are unaffected (Obsidian resolves `[[Note Name]]` by name, not path), so no link rewriting is required.

## Conventions

### Frontmatter schema

Every note carries enough for an agent to judge relevance:

```yaml
type: knowledge | context | spec | reference | moc | daily
project: edbot-v2        # omit or "global" for knowledge/reference
status: draft | active | evergreen | shipped | archived
created: 2026-06-30
updated: 2026-06-30
tags: [...]
```

Existing notes already use `title/aliases/tags/created/status/source`; the reorg **adds** `type` and `project` and keeps the rest. `status` gains `draft/active/shipped/archived` values for specs alongside the existing `evergreen`.

### The separation rule (core guardrail)

- **`context/`** = durable understanding — *how things are and why*. Stable, evergreen-leaning.
- **`specs/`** = intent to change — *what to build*, with acceptance criteria and affected repos/files. Each spec links to the `context/` it relies on via `[[wikilinks]]`.
- **Tracking/status is NOT stored in the vault** — ClickUp remains the source of truth for task status (preserves the merge-conflict lesson).

### Spec template shape

`Goal · Background (links to context/) · Requirements · Non-goals · Acceptance criteria · Affected repos/files · Open questions`

### Templates provided in `90-templates/`

- `context-note.md` — frontmatter + section skeleton for durable understanding.
- `spec.md` — the spec shape above.
- `project-index.md` — a project MOC skeleton (cluster table, related, sources).
- `knowledge-note.md` — evergreen note skeleton.

## Root `CLAUDE.md` contents

The root `CLAUDE.md` instructs any agent pointed at the vault:

1. **What this is** — a context + spec knowledge vault, not a code repo. Plain Markdown, Obsidian-native.
2. **Folder map** — the structure above, one line each.
3. **Where to start** — root → relevant `20-projects/<project>/<project>.md` MOC → that project's `context/` for orientation, then `specs/` for intent. Pull from `10-knowledge/` for cross-project conventions.
4. **Frontmatter schema** — how to filter notes by `type`/`project`/`status`.
5. **The separation rule** — context = understanding; specs = what to build; status lives in ClickUp, not here.
6. **Writing rules for agents** — new durable understanding → `context/`; new build intent → `specs/` using the template; capture-only → `00-inbox/`; never write task status into notes.
7. **Obsidian conventions** — link with `[[wikilinks]]`, one idea per note, properties in YAML frontmatter.

## Out of scope (YAGNI)

- Per-project `CLAUDE.md` files (root only for now).
- Daily notes / `20-daily` workflow (can be added later if used).
- Link-rewriting tooling (Obsidian resolves links by name).
- Automated frontmatter migration scripts — backfilling `type`/`project` is a small manual/templated pass.
- Any change to how the Edbot code repos or ClickUp work.

## Success criteria

- Top level cleanly separates reusable knowledge (`10-knowledge`) from projects (`20-projects`), and within a project, context from specs.
- All 25 existing notes are relocated via `git mv` with history intact and wikilinks still resolving in Obsidian.
- A root `CLAUDE.md` exists that lets an agent, given only the vault, correctly find project context and know where specs go.
- `90-templates/` contains the four templates.
- `README.md` reflects the new layout.
