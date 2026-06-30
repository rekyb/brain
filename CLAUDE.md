# CLAUDE.md — How to use this vault

This is **`brain`**, an Obsidian knowledge vault. It holds **context** (durable understanding) and **specs** (what to build). It is **not** a code repository — there is no app to run here. Every note is plain Markdown with YAML frontmatter, linked with `[[wikilinks]]`.

## What you're here for

Agents read this vault as **background context** while working in separate code repos (e.g. the Edbot app repos). Use it to orient: architecture, conventions, design knowledge, and per-project understanding.

## Folder map

| Path | Holds |
| ---- | ----- |
| `00-inbox/` | Quick capture, unprocessed. Triage out of here. |
| `10-knowledge/` | Reusable, **project-agnostic** evergreen notes. `design-systems/`, `atomic-design/`, `agentic-ai/`. |
| `20-projects/<project>/` | Per-project area. `<project>.md` is the MOC/index. |
| `20-projects/<project>/context/` | Durable **understanding** of the project — how it works and why. |
| `20-projects/<project>/specs/` | Authoritative **specs** — what to build, with acceptance criteria. |
| `40-references/` | External sources, clippings, quotes. |
| `90-templates/` | Note templates: `knowledge-note`, `context-note`, `spec`, `project-index`. |
| `attachments/` | Binaries (images, PDFs). Created on first use. |
| `docs/superpowers/` | Claude Code workflow artifacts (design specs, plans). **Not vault content** — historical; do not read as current state. Graph-isolated: no `[[wikilinks]]` to/from real notes. |

## Where to start (reading)

1. For a project, open `20-projects/<project>/<project>.md` (the MOC) — it maps the cluster.
2. Read that project's `context/` to understand the system.
3. Read `specs/` for current build intent.
4. Pull cross-project conventions from `10-knowledge/`.

Filter by frontmatter when scanning: `type`, `project`, `status`.

## Frontmatter schema

```yaml
type: knowledge | context | spec | reference | moc | daily
project: <slug>        # omit or "global" for knowledge & reference
status: draft | active | evergreen | shipped | archived
created: YYYY-MM-DD
updated: YYYY-MM-DD    # optional; add/bump on first edit, not required at creation
tags: [...]
```

## The separation rule (important)

- **context/** = how things *are* and *why*. Durable understanding. Evergreen-leaning.
- **specs/** = what to *build* — requirements, acceptance criteria, affected repos/files. Each spec links to the `context/` it relies on.
- **Status/tracking does NOT live in this vault.** Task status is tracked in **ClickUp**. Writing live trackers into markdown previously caused cross-machine merge conflicts — do not reintroduce them.

## Writing rules (when you add notes)

- New durable understanding → `20-projects/<project>/context/` using `90-templates/context-note.md`.
- New build intent → `20-projects/<project>/specs/` using `90-templates/spec.md`.
- Reusable, project-agnostic knowledge → `10-knowledge/<area>/` using `90-templates/knowledge-note.md`.
- Rough capture → `00-inbox/`; refine and file it later.
- Always set `type` and (for project notes) `project`; bump `updated:` when you edit.
- Link related notes with `[[wikilinks]]`. One idea per note. Keep notes focused.
- Never put task status/progress in a note — that belongs in ClickUp/Slack.
- Superpowers artifacts (`docs/superpowers/`) are historical process artifacts, not durable vault content — never add `[[wikilinks]]` connecting them to or from real notes, and never reference them from MOCs or any other note. They must remain isolated from the knowledge graph.
