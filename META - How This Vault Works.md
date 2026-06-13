# How This Vault Works

## The Core Idea

This vault uses a flat file structure — almost everything lives at the root, not in nested folders. Notes are organized by a short uppercase **prefix** in the filename. Dynamic **MOC** (Map of Content) files automatically surface all notes under a given prefix, so there's nothing to manually maintain.

No folders means no agonizing over where a note "belongs." The prefix is the category.

---

## Prefixes

Every permanent note starts with a prefix:

```
PREFIX - Note Title.md
```

Examples:
- `HOW - De-skunk Mixture.md`
- `HOW - Set Up a New Mac.md`
- `HOW - De-skunk Mixture.md`
- `HOW - Set Up a New Mac.md`

**Starter prefixes in this vault:**

| Prefix | Category |
|--------|----------|
| `HOW` | Step-by-step procedural notes — how to do a specific thing |

**Adding a new prefix:** Pick a short (2-5 char) uppercase abbreviation. Create one `.base` MOC file for it (copy any existing one, change the `startsWith` filter). That's it.

---

## MOCs (Maps of Content)

Each `PREFIX - MOC - Name.base` file is a live query — it automatically shows every note with that prefix. You never need to update it manually.

`MOC Index.base` shows all MOCs in one place. Use it as your home base.

**Filter approach:** MOCs use two filters:
```yaml
- file.name.startsWith("PREFIX -")
- file.ext == "md"
```
`startsWith` is enforced by the naming convention — zero overhead per note. `ext == "md"` keeps `.base` files out of the results. Tags were considered as an alternative (they'd allow a note to appear in multiple MOCs) but require adding a tag to every note, which drifts. One note, one MOC is the right constraint for a personal vault.

---

## Capture: Two Streams

### 1. Daily Notes (`Daily Notes/` folder)
A logbook and scratchpad for the day. Open with `Cmd+D`.
- **Log** — what you did, decisions made, things that happened. The record.
- **Notes** — passing observations, things that crossed your mind, context tied to the day. Not meant to survive long-term.

### 2. Interstitial Notes (root, timestamped)
For when a thought deserves to exist on its own, not just as a footnote to the day. Use `Cmd+U`.
- Title format: `YYYY-MM-DD HHmm - Brief subject`
- Use when the idea is the point — something you want to think through, reference later, or potentially promote to a permanent note.
- These are not permanent notes either — process within 1-2 weeks or delete.

**The distinction:** daily note Notes = "this happened today." Interstitial = "this thought stands on its own."

**The rule:** if an interstitial note is still relevant after a week, give it a prefix and move it to root as a permanent note.

---

## Clippings (`Clippings/` folder)

Web clips from the Obsidian Web Clipper browser extension land here automatically. Treat them as an inbox:
- High-value clips: promote to a prefixed note or just leave with a tag
- Everything else: delete within a week or two

Most clips don't survive the week. That's fine — the point is quick capture, not permanent storage.

---

## Weekly Review (15-20 min)

1. **Process interstitial notes** — promote keepers to prefixed notes, delete the rest
2. **Process Daily Notes** — pull out anything worth keeping permanently
3. **Triage Clippings** — delete aggressively, promote the rare keeper
4. **Scan MOC Index** — make sure nothing feels out of place

---

## Folders

The root is for your notes only. Folders are reserved for external/imported content and system files:

| Folder | Purpose |
|--------|---------|
| `Daily Notes/` | Daily log notes, structured by date |
| `Clippings/` | Web clips from the Obsidian Web Clipper |
| `Attachments/` | Images and files, managed automatically by Obsidian |
| `Templates/` | Note templates |

If you're creating a note yourself, it lives at root with a prefix. If it's someone else's content, it goes in a folder. Add new folders sparingly and only for the same reason — high-volume imported/external content that doesn't belong in the main namespace.

---

## Naming Rules

- Dates always as `YYYY-MM-DD`
- Prefix always uppercase, followed by ` - ` (space-dash-space)
- Tag and folder names pluralized (`#people` not `#person`)
- Keep note titles descriptive but concise

---

## Plugins

### Core Plugins (built into Obsidian)

These are configured in `.obsidian/core-plugins.json` and enabled by default in this template:

| Plugin | Purpose |
|--------|---------|
| Daily Notes | Creates dated notes in `Daily Notes/` folder |
| Templates | Populates new notes from `Templates/` |
| Unique Note Creator | Creates timestamped interstitial notes at root |
| Bases | Powers the `.base` MOC files |
| Quick Switcher | `Cmd+O` fast file navigation |
| Command Palette | `Cmd+P` access to all commands |
| Backlinks | Shows what links to the current note |
| File Recovery | Auto-snapshots for recovery |

Intentionally disabled: Graph View, Canvas, Outgoing Links, Tag Pane, Workspaces, Slides, Sync. Re-enable any you want in Settings → Core Plugins.

### Community Plugins

| Plugin | Purpose | Install |
|--------|---------|---------|
| [Harper](https://github.com/elijah-potter/harper) | Grammar and spell checking | Settings → Community Plugins → Browse → search "Harper" |

**Note:** Community plugins require a one-time manual enable in Settings → Community Plugins after installing. The config in `.obsidian/plugins/harper/data.json` pre-sets Harper to American English with `BoringWords` and `SpelledNumbers` rules enabled.

---

## Hotkeys

Only two custom bindings are set. Everything else is the Obsidian default.

| Hotkey | Action | Why custom |
|--------|--------|------------|
| `Cmd+D` | Open today's daily note | No Obsidian default |
| `Cmd+U` | New interstitial note (Unique Note Creator) | No Obsidian default |

**Key Obsidian defaults to know:**

| Hotkey | Action |
|--------|--------|
| `Cmd+P` | Command palette |
| `Cmd+O` | Quick switcher |
| `Cmd+N` | New note |
| `Cmd+K` | Insert link |
| `Cmd+B` | Bold |
| `Cmd+I` | Italic |
| `Cmd+F` | Search in current note |
| `Cmd+Shift+F` | Search all notes |

---

## Adding a New Category

1. Pick a prefix (e.g. `FIN` for finance)
2. Create `FIN - MOC - Finance.base` with:
```yaml
views:
  - type: table
    name: Table
    filters:
      and:
        - file.name.startsWith("FIN -")
    order:
      - file.name
```
3. Start creating notes with the `FIN -` prefix
4. Update this file's prefix table above
