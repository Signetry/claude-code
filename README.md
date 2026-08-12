# umbra-claude-code

> **Copyright (c) 2026 Binay Dalai. All rights reserved.**
> This repository is strictly for viewing and contributing to the original project. You may not use, copy, modify, distribute, or commercialize this code for your own personal or commercial projects without explicit written permission. Only the original author retains the right to use and monetize this project.


**Govern Claude Code's changes in real time — with signed receipts.**


The Claude Code plugin for [umbra-core](https://github.com/Signetry/core):
Umbra decides how much authority an agent's change has earned and proves it. This
plugin brings that governance *inside Claude Code* — enforced by deterministic
code, never by the model itself (an agent can't approve its own change).

Part of the [Umbra platform](https://github.com/Signetry/signetry).

> Prerequisite: `pip install "umbra-core @ git+https://github.com/Signetry/core@v0.5.4"` and a `.umbra/admission.yaml` in
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

## Scan & fix (from the same `umbra-core`)

Beyond the editor guard, `umbra-core` can find vulnerabilities and govern the fix:

```bash
umbra scan .                 # SAST over the repo (7 languages, SARIF), offline & free
umbra scan . --fix --fix-agent claude-code   # draft a governed fix → admission → signed receipt
```

`--fix` is **bring-your-own-key** (your `ANTHROPIC_API_KEY`, never shared, redacted
from every artifact) and opens **branch-only** PRs — never merges. See
[umbra-core: AUTOFIX_SETUP.md](https://github.com/Signetry/core/blob/main/docs/AUTOFIX_SETUP.md).

## Guarantees

- Fail-closed on admit; the soft guard may fail open only with a loud `INACTIVE`.
- Never auto-merges. `auto_merge` is always false — a human merges.
- Governance logic lives in `umbra-core`; this plugin never reimplements policy.

See [SECURITY.md](SECURITY.md) · [PRIVACY.md](PRIVACY.md) ·
[umbrella overview](https://github.com/Signetry/signetry).

## License

**Copyright (c) 2026 Binay Dalai. All rights reserved.** This code is not open source. You may not use, copy, modify, distribute, or commercialize it for your own personal or commercial purposes without explicit written permission from the author, who alone retains the right to use and monetize this project. See [CONTRIBUTING.md](CONTRIBUTING.md).
