# Obsidian Vault Template

A minimal Obsidian vault template using a flat file structure, prefix-based organization, and auto-updating MOC files.

## Setup

1. Clone or download this repo
2. Open the folder as a vault in Obsidian (`Open folder as vault`)
3. **Install [Concordance](https://github.com/nsyout/concordance)** — required for the MOC system. It is not in the official Community Plugins directory; install via [BRAT](https://github.com/TfTHacker/obsidian42-brat) (add beta plugin `nsyout/concordance`) or by unzipping a release into `.obsidian/plugins/concordance/`. Then enable it in Settings → Community Plugins.
4. Install the [Harper](https://github.com/elijah-potter/harper) community plugin for grammar/spellcheck (Settings → Community Plugins → Browse → "Harper").

## Plugin dependencies

| Plugin | Required? | What breaks without it |
|--------|-----------|------------------------|
| [Concordance](https://github.com/nsyout/concordance) | **Yes** | MOC index lists stay empty — the `%% concordance:start/end %%` blocks won't auto-populate |
| [Harper](https://github.com/elijah-potter/harper) | No | No grammar/spell checking |

Plugin config (`data.json`) is pre-committed under `.obsidian/plugins/<id>/` so settings apply on first enable. Plugin binaries are **not** committed — you install those yourself.

## How it works

See `META - How This Vault Works.md` inside the vault.
