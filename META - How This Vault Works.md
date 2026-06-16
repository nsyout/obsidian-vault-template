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

**Prefix length:** 1–4 uppercase characters, max. Short enough that you can type it fast and it doesn't dominate the filename. If it's longer than 4, pick a tighter abbreviation. See "Adding a New Category" below for how to scaffold the MOC file.

---

## MOCs (Maps of Content)

Each `PREFIX - MOC - Name.md` file is an index of every note with that prefix. The list between the `%% concordance:start %%` and `%% concordance:end %%` markers is regenerated automatically by the [Concordance plugin](https://github.com/nsyout/concordance) — you never edit it by hand.

[[MOC Index.base]] shows all MOCs in one place. Use it as your home base. (It's a `.base` because it's a one-shot filter over filenames, not a managed index.)

**How a MOC works:**

```markdown
#moc

## Index
%% concordance:start %%
%% concordance:end %%
```

Concordance watches notes whose filename starts with the MOC's prefix (e.g. `HOW -`) and writes them as `[[wiki links]]` between the markers. The `#moc` tag is what `MOC Index.base`'s tag view picks up. Why markdown MOCs over pure Bases queries? Markdown MOCs travel — they're plain text wiki-link lists that work in any tool, sync cleanly through git, and let you add prose, sub-headings, or pinned links above the auto-generated section. Bases queries only render inside Obsidian.

> **Requires the Concordance community plugin.** Without it, the marker blocks stay empty. See the Plugins section below for install steps.

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
| [Concordance](https://github.com/nsyout/concordance) | **Required.** Auto-populates MOC index lists between `%% concordance:start/end %%` markers | Install via [BRAT](https://github.com/TfTHacker/obsidian42-brat) → add beta plugin `nsyout/concordance`, or download a release and unzip into `.obsidian/plugins/concordance/` |
| [Harper](https://github.com/elijah-potter/harper) | Grammar and spell checking | Settings → Community Plugins → Browse → search "Harper" |

**Note:** Community plugins require a one-time manual enable in Settings → Community Plugins after installing. The pre-shipped `data.json` files configure each plugin: Harper is set to American English with `BoringWords` and `SpelledNumbers` enabled; Concordance is set to the `PREFIX - MOC - Name` convention and the marker pair this vault uses.

**Without Concordance**, MOC files still exist but their index sections stay empty. You can either install the plugin or rewrite the MOCs as `.base` filter files (see [the personal vault README](https://github.com/nsyout/obsidian-vault-template) for the prior pure-Bases approach).

#### How plugin settings work

Obsidian stores every community plugin's settings in `.obsidian/plugins/<plugin-id>/data.json`. The plugin reads this file when it's enabled, and writes back to it whenever you change a setting in Settings → Community Plugins → (plugin). It's plain JSON, so you can edit it directly or commit it to version control to share config across machines/clones. This template ships the `data.json` for both Concordance and Harper so they're pre-configured the moment you enable them — there's no separate setup step.

The plugin binaries (`main.js`, `manifest.json`, `styles.css`) are **not** committed. You install those once via BRAT or a release zip; afterward they live alongside `data.json` in the same folder.

#### Concordance settings (`.obsidian/plugins/concordance/data.json`)

```json
{
  "indexFilenameTemplate": "{PREFIX} - MOC - {DISPLAY_NAME}",
  "childFilenamePrefixTemplate": "{PREFIX} - ",
  "startMarker": "%% concordance:start %%",
  "endMarker": "%% concordance:end %%",
  "autoIndexHeading": "Index",
  "missingBlockBehavior": "ask",
  "excludedFolders": [],
  "excludedFilenameTerms": []
}
```

| Key | What it does | Why this value |
|-----|--------------|----------------|
| `indexFilenameTemplate` | Filename pattern that identifies a MOC. `{PREFIX}` is the uppercase abbreviation; `{DISPLAY_NAME}` is the human-readable label after `MOC -`. | Matches this vault's `PREFIX - MOC - Name.md` convention so Concordance can pair each MOC with its prefix. |
| `childFilenamePrefixTemplate` | Pattern a note must start with to be indexed by a given MOC. | `HOW -` notes get listed in the `HOW - MOC - …` file, etc. |
| `startMarker` / `endMarker` | The literal HTML-comment strings that fence the auto-managed list inside a MOC. | Comment syntax means the markers are invisible in reading view but stable for the plugin to find. |
| `autoIndexHeading` | The heading Concordance inserts above the markers if you let it scaffold a missing block. | We always put the index under a `## Index` heading. |
| `missingBlockBehavior` | What to do when a MOC has no marker block: `ask`, `insert`, or `skip`. | `ask` prevents the plugin from silently editing files you weren't expecting it to. |
| `excludedFolders` | Folders whose contents should be ignored when building indexes. | Empty — `Daily Notes/`, `Clippings/`, etc. already won't match the prefix pattern, so no exclusions needed. |
| `excludedFilenameTerms` | Substrings that exclude a file from indexing even if it matches the prefix. | Empty for the same reason. |

To change any of these, edit `data.json` directly or use Settings → Community Plugins → Concordance — both write to the same file.

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

1. Pick a prefix — 1–4 uppercase characters (e.g. `FIN` for finance)
2. Create `FIN - MOC - Finance.md` with:
```markdown
#moc

## Index
%% concordance:start %%
%% concordance:end %%
```
3. Start creating notes with the `FIN -` prefix — Concordance will fill the marker block as you go
4. Update this file's prefix table above
