# 🧠 brain

A personal knowledge vault of my [Obsidian](https://obsidian.md) "second brain" for capturing notes, ideas, references, and connections over time.

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

## Suggested structure

The vault starts empty by design — let structure emerge from use rather than forcing it up front. A lightweight convention that scales well:

```
brain/
├── 00-inbox/      # Quick capture; unprocessed notes land here first
├── 10-notes/      # Evergreen / permanent notes (the core of the brain)
├── 20-daily/      # Daily notes (journal, log, scratch)
├── 30-projects/   # Active work, goals, project-specific notes
├── 40-references/ # Sources: books, articles, links, quotes
├── 90-templates/  # Note templates
└── attachments/   # Images, PDFs, and other binaries
```

Folders are optional in Obsidian — links and tags do most of the organizing. Use folders only as far as they help.

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
