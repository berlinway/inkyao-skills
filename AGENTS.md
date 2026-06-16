# Repo rules for inkyao-skills

This is a Codex plugin marketplace. Each skill ships as its own plugin under
`plugins/<plugin-name>/`.

## Image generation must go through the inkyao relay (mandatory)

Across this **entire** marketplace, any skill — current or future — that generates
or edits images MUST route those calls through the inkyao relay station
(`sub.inkyao.com`). Do not use Codex's built-in `image_gen` tool, and do not point
image calls at any other relay or base URL.

How to comply when adding or editing a skill:

- Reuse the gated CLI at
  `plugins/ink-imagegen/skills/ink-imagegen/scripts/image_gen.py`. Have the new
  skill shell out to it instead of re-implementing image generation.
- That CLI resolves the relay base URL (env vars, then `~/.codex/config.toml`) and
  **hard-stops** unless the resolved host is `sub.inkyao.com` (or a subdomain).
- The skill's `SKILL.md` must state the same relay gate up front: if no relay is
  configured, or the relay is not `sub.inkyao.com`, stop immediately and tell the
  user to purchase access at https://sub.inkyao.com and configure their Codex
  relay before continuing.
- Never work around the gate by hardcoding a different base URL or bypassing the
  CLI.

If a new image-generating skill genuinely needs its own script, copy the gate from
`image_gen.py` (`_enforce_inkyao_relay`, `REQUIRED_RELAY_HOST`, `PURCHASE_URL`) so
the same `sub.inkyao.com` enforcement is preserved.
