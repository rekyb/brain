# 🧠 brain

A personal knowledge vault of my Obsidian vault for capturing notes, ideas, references, and connections over time.

This repo *is* the vault: open the folder directly in Obsidian (**Open folder as vault**) and everything works out of the box.

---

## What this is

A plain-text, Markdown-first knowledge base. Notes are linked together with `[[wikilinks]]`, surfaced through the graph and backlinks, and version-controlled with Git so nothing is ever lost.

No proprietary format — every note is a `.md` file you can read, grep, and edit with any tool.

## Getting started

1. Install [Obsidian](https://obsidian.md/download).
2. **Open folder as vault** → select this `brain` directory.
3. Start writing. Link notes with `[[Note Name]]` and watch the graph fill in.

To clone and sync elsewhere:

```bash
git clone <this-repo-url> brain
cd brain
# then open the folder as a vault in Obsidian
```

## Structure

The vault is organized for both human browsing and use as **context for context- & spec-driven agentic AI development**: a reusable knowledge base plus per-project areas that separate durable *context* from *specs*.

```
brain/
├── CLAUDE.md         # how AI agents read & use this vault
├── 00-inbox/         # quick capture, unprocessed
├── 10-knowledge/     # reusable, project-agnostic evergreen notes
│   ├── design-systems/
│   ├── atomic-design/
│   └── agentic-ai/
├── 20-projects/      # per-project context + specs
│   └── edbot-v2/
│       ├── edbot-v2.md   # project index (MOC)
│       ├── context/      # durable understanding
│       └── specs/        # what to build
├── 40-references/    # external sources, clippings
├── 90-templates/     # note / context / spec / project templates
└── attachments/      # images, PDFs, binaries (created on first use)
```

> **For AI agents:** see [`CLAUDE.md`](CLAUDE.md) — it explains how to navigate the vault, the frontmatter schema, and the context-vs-spec rule (status lives in ClickUp, not here).

Folders carry real meaning here, but links and tags still do much of the organizing. Use folders only as far as they help.

## Conventions

- **Links over folders** — connect notes with `[[wikilinks]]`; let the graph reveal relationships.
- **Tags for facets** — use `#tag` for cross-cutting themes (`#idea`, `#todo`, `#reference`).
- **Properties for metadata** — YAML frontmatter (`---`) for status, dates, aliases, and structured fields.
- **One idea per note** — small, atomic, well-titled notes link and reuse better than long ones.
- **Capture first, organize later** — drop quick thoughts in the inbox; refine and link them afterward.

## Enabled Obsidian features

Configured in `.obsidian/` (core plugins):

- **Graph view** — visualize how notes connect
- **Backlinks & outgoing links** — see what references each note
- **Canvas** — spatial, freeform boards
- **Daily notes** — a fresh note per day
- **Templates** — reusable note scaffolds
- **Properties & Bases** — structured metadata and database-style views
- **Tag pane**, **Outline**, **Bookmarks**, **Word count**
- **Sync** — Obsidian Sync enabled alongside Git

## Version control

This vault is a Git repository. Commit regularly to keep history and enable sync across machines.

```bash
git add -A
git commit -m "notes: <what changed>"
```

The `.obsidian/` folder is tracked so settings travel with the vault. Workspace-local state (`workspace.json`) can be ignored if it causes churn across devices.

---

*Built with [Obsidian](https://obsidian.md). Plain Markdown, owned forever.*
