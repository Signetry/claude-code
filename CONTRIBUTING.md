# Contributing to umbra-claude-code

Thanks for contributing! This is an **integration** for the Umbra platform — a thin
adapter that governs a coding agent through the [`umbra-core`](https://github.com/bkd-dotcom/umbra-core)
admission pipeline. It never reimplements governance policy.

## Good contributions

- Fixes / improvements to the guard, hook, MCP wiring, or setup docs for this
  integration.
- Better install ergonomics, examples, or troubleshooting.

## Ground rules

- **Governance logic stays in `umbra-core`** — this repo adapts an agent to it,
  nothing more. Depend on a pinned `umbra-core` (`>=0.5.0`).
- **Never auto-merge.** `auto_merge` is always false; a human merges.
- Fail closed on the hard gate; a soft guard may fail open only with a loud
  `INACTIVE`.
- Be kind — see [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md). Report security issues via
  [private advisory](https://github.com/bkd-dotcom/umbra-claude-code/security/advisories/new), not a
  public issue.

New to Umbra? Start at the [umbrella overview](https://github.com/bkd-dotcom/umbra-umbrella).
