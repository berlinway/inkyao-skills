# inkyao-skills

A [Codex](https://developers.openai.com/codex/plugins) plugin marketplace. Each skill is
distributed as its own plugin, so users can install just the ones they want.

## Install the marketplace

In the Codex app: **Plugins → Add plugin marketplace**

| Field        | Value                           |
| ------------ | ------------------------------- |
| Source       | `berlinway/inkyao-skills`       |
| Git ref      | `main`                          |
| Sparse paths | `.agents/plugins` and `plugins` |

Or from the CLI:

```bash
codex plugin marketplace add berlinway/inkyao-skills --ref main
```

Then install an individual plugin:

```bash
codex plugin install ink-imagegen
```

## Available plugins

| Plugin         | Description                                                        |
| -------------- | ----------------------------------------------------------------- |
| `ink-imagegen` | Generate/edit images through an OpenAI-compatible relay image API. |

## Repo layout

```
.agents/plugins/marketplace.json   # marketplace catalogue (discovery entry)
plugins/
  ink-imagegen/
    .codex-plugin/plugin.json      # required plugin manifest
    skills/ink-imagegen/SKILL.md   # the skill itself (+ scripts, references, assets)
    assets/                        # plugin icons
```

## Adding a new skill

1. Create `plugins/<plugin-name>/.codex-plugin/plugin.json`.
2. Put the skill at `plugins/<plugin-name>/skills/<skill-name>/SKILL.md` (with any
   `scripts/`, `references/`, `assets/` it needs).
3. Add a new entry to the `plugins` array in `.agents/plugins/marketplace.json`,
   pointing `source.path` at `./plugins/<plugin-name>`.
4. Commit and push to `main`.
