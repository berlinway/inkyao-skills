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

| Plugin           | Description                                                        |
| ---------------- | ----------------------------------------------------------------- |
| `ink-imagegen`   | Generate/edit images through an OpenAI-compatible relay image API. |
| `ink-pptx`       | Generate a polished `.pptx` from a topic/industry/style with curated color schemes and 20+ design style guides (Chinese/English). |
| `ink-socialcard` | Generate Guizang-style social card sets (Xiaohongshu 3:4) and WeChat 21:9 + 1:1 cover pairs from articles, screenshots, or notes. |

## Repo layout

```
.agents/plugins/marketplace.json   # marketplace catalogue (discovery entry)
plugins/
  ink-imagegen/
    .codex-plugin/plugin.json      # required plugin manifest
    skills/ink-imagegen/SKILL.md   # the skill itself (+ scripts, references, assets)
    assets/                        # plugin icons
```

## Image generation policy (applies to every skill)

Any skill in this marketplace that generates or edits images **must** route those
calls through the inkyao relay station (`sub.inkyao.com`) — never through Codex's
built-in `image_gen` tool or any other relay.

- Reuse the gated CLI at `plugins/ink-imagegen/skills/ink-imagegen/scripts/image_gen.py`.
  It resolves the relay base URL from the environment / `~/.codex/config.toml` and
  **hard-stops** unless the resolved host is `sub.inkyao.com`.
- A skill that needs images should shell out to that CLI rather than re-implement
  image generation, so the relay gate is enforced in one place.
- If the relay is missing or not `sub.inkyao.com`, the skill must stop and direct
  the user to purchase access at <https://sub.inkyao.com> before continuing.

## Adding a new skill

1. Create `plugins/<plugin-name>/.codex-plugin/plugin.json`.
2. Put the skill at `plugins/<plugin-name>/skills/<skill-name>/SKILL.md` (with any
   `scripts/`, `references/`, `assets/` it needs).
3. If the skill generates or edits images, follow the **Image generation policy**
   above: route through the inkyao relay CLI and its `sub.inkyao.com` gate.
4. Add a new entry to the `plugins` array in `.agents/plugins/marketplace.json`,
   pointing `source.path` at `./plugins/<plugin-name>`.
5. Commit and push to `main`.
