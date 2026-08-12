# signetry-claude-code

> **Copyright (c) 2026 Binay Dalai. All rights reserved.**
> This repository is strictly for viewing and contributing to the original project. You may not use, copy, modify, distribute, or commercialize this code for your own personal or commercial projects without explicit written permission. Only the original author retains the right to use and monetize this project.


**Govern Claude Code's changes in real time — with signed receipts.**


The Claude Code plugin for [signetry-core](https://github.com/Signetry/core):
Signetry decides how much authority an agent's change has earned and proves it. This
plugin brings that governance *inside Claude Code* — enforced by deterministic
code, never by the model itself (an agent can't approve its own change).

Part of the [Signetry platform](https://github.com/Signetry/signetry).

> Prerequisite: `pip install "signetry-core @ git+https://github.com/Signetry/core@v0.6.0"` and a `.signetry/admission.yaml` in
> your repo (a conservative default applies without one).

## What it does

A **`PreToolUse` hook** runs before every `Edit` / `Write` / `Bash` and **blocks**
out-of-scope or forbidden actions before they happen — using `signetry guard`
(deterministic, not the model). It bundles the Signetry **MCP server** and an
**`/signetry:admit`** skill for on-demand full admission with a signed receipt.

- Agent tries to edit `deploy.yml` / `.env` / a secret, or run `curl … | bash` /
  `git push` → **blocked** with a reason, before it happens.
- Agent edits an in-scope file → allowed silently.
- `/signetry:admit` → runs the full admission pipeline and returns an Admission
  Decision Pack + signed receipt.

## Install

```
/plugin marketplace add bkd-dotcom/signetry-claude-code
/plugin install signetry@signetry-claude-code
```

Or test locally: `claude --plugin-dir ./signetry`

## Layout

- `signetry/.claude-plugin/plugin.json` — plugin manifest
- `signetry/hooks/` — the `PreToolUse` guard + session-start hooks
- `signetry/scripts/signetry-mcp.sh` — launches the Signetry MCP server
- `signetry/skills/admit/SKILL.md` — the `/signetry:admit` skill
- `signetry/.mcp.json` — MCP server registration

## Scan & fix (from the same `signetry-core`)

Beyond the editor guard, `signetry-core` can find vulnerabilities and govern the fix:

```bash
signetry scan .                 # SAST over the repo (7 languages, SARIF), offline & free
signetry scan . --fix --fix-agent claude-code   # draft a governed fix → admission → signed receipt
```

`--fix` is **bring-your-own-key** (your `ANTHROPIC_API_KEY`, never shared, redacted
from every artifact) and opens **branch-only** PRs — never merges. See
[signetry-core: AUTOFIX_SETUP.md](https://github.com/Signetry/core/blob/main/docs/AUTOFIX_SETUP.md).

## Guarantees

- Fail-closed on admit; the soft guard may fail open only with a loud `INACTIVE`.
- Never auto-merges. `auto_merge` is always false — a human merges.
- Governance logic lives in `signetry-core`; this plugin never reimplements policy.

See [SECURITY.md](SECURITY.md) · [PRIVACY.md](PRIVACY.md) ·
[umbrella overview](https://github.com/Signetry/signetry).

## License

**Copyright (c) 2026 Binay Dalai. All rights reserved.** This code is not open source. You may not use, copy, modify, distribute, or commercialize it for your own personal or commercial purposes without explicit written permission from the author, who alone retains the right to use and monetize this project. See [CONTRIBUTING.md](CONTRIBUTING.md).
