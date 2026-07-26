# umbra-claude-code

**Govern Claude Code's changes in real time — with signed receipts.**

The Claude Code plugin for [umbra-core](https://github.com/bkd-dotcom/umbra-core):
Umbra decides how much authority an agent's change has earned and proves it. This
plugin brings that governance *inside Claude Code* — enforced by deterministic
code, never by the model itself (an agent can't approve its own change).

Part of the [Umbra platform](https://github.com/bkd-dotcom/umbra-umbrella).

> Prerequisite: `pip install "umbra-core>=0.3.0"` and a `.umbra/admission.yaml` in
> your repo (a conservative default applies without one).

## What it does

A **`PreToolUse` hook** runs before every `Edit` / `Write` / `Bash` and **blocks**
out-of-scope or forbidden actions before they happen — using `umbra guard`
(deterministic, not the model). It bundles the Umbra **MCP server** and an
**`/umbra:admit`** skill for on-demand full admission with a signed receipt.

- Agent tries to edit `deploy.yml` / `.env` / a secret, or run `curl … | bash` /
  `git push` → **blocked** with a reason, before it happens.
- Agent edits an in-scope file → allowed silently.
- `/umbra:admit` → runs the full admission pipeline and returns an Admission
  Decision Pack + signed receipt.

## Install

```
/plugin marketplace add bkd-dotcom/umbra-claude-code
/plugin install umbra@umbra-claude-code
```

Or test locally: `claude --plugin-dir ./umbra`

## Layout

- `umbra/.claude-plugin/plugin.json` — plugin manifest
- `umbra/hooks/` — the `PreToolUse` guard + session-start hooks
- `umbra/scripts/umbra-mcp.sh` — launches the Umbra MCP server
- `umbra/skills/admit/SKILL.md` — the `/umbra:admit` skill
- `umbra/.mcp.json` — MCP server registration

## Guarantees

- Fail-closed on admit; the soft guard may fail open only with a loud `INACTIVE`.
- Never auto-merges. `auto_merge` is always false — a human merges.
- Governance logic lives in `umbra-core`; this plugin never reimplements policy.

See [SECURITY.md](SECURITY.md) · [PRIVACY.md](PRIVACY.md) ·
[umbrella overview](https://github.com/bkd-dotcom/umbra-umbrella).

## License

[MIT](LICENSE) © 2026 Binay Dalai.
