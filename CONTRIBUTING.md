# Contributing

Thanks for wanting to improve the **Signetry Claude Code plugin**. This repository
is an integration surface — the plugin that brings `signetry-core`'s governance
inside Claude Code — and it is [Apache-2.0](LICENSE): use it, fork it, ship it
commercially, no strings.

## What the licence lets you do

Apache-2.0 gives you a patent grant and the right to use, copy, modify,
distribute, and commercialize this plugin, including in closed-source and
commercial products. You do not need our permission and you do not owe us
anything. Keep the `LICENSE` and the attribution notices when you redistribute,
and note your changes — that is the whole obligation.

This repo is part of Signetry's
[open-core model](https://github.com/Signetry/signetry/blob/main/LICENSING.md):
every integration (this plugin, the [GitHub Action](https://github.com/Signetry/action),
the other editor and agent plugins, the pre-commit guard, the eval suite) is
Apache-2.0, while the engine ([`Signetry/core`](https://github.com/Signetry/core))
is source-available under BUSL-1.1 and converts to Apache-2.0 on 2030-08-31.

## The CLA still applies — and why

A PR **cannot be merged** until you sign the
[Contributor License Agreement](CLA.md). It is enforced by a bot: when you open a
pull request, the **CLA Assistant** check asks you to reply on the PR with exactly:

```
I have read the CLA Document and I hereby sign the CLA
```

Your acceptance is recorded in `signatures/cla.json`.

The CLA is not about withholding rights from you — Apache-2.0 already grants you
everything above, and signing does not take it away. It exists because code moves
across the open-core line. A well-built adapter that starts here as Apache-2.0
may later belong in the BUSL-1.1 engine, and Signetry needs the relicensing
rights to move it without tracking down every past contributor for permission.
It also lets us dual-license and defend the project if that is ever necessary.

## Getting started

There is no build step and no compiled artifact. The plugin is **bash hooks plus
JSON manifests plus a skill markdown file** under `signetry/`:

- `signetry/.claude-plugin/plugin.json` — plugin manifest
- `signetry/hooks/` — the `PreToolUse` guard (`signetry-guard.sh`), the
  `SessionStart` hook, and the shared resolver `signetry-lib.sh`
- `signetry/hooks/hooks.json` — which hook fires on which tool matcher
- `signetry/scripts/signetry-mcp.sh` — launches the Signetry MCP server
- `signetry/skills/admit/SKILL.md` — the `/signetry:admit` skill
- `signetry/.mcp.json` — MCP server registration
- `.claude-plugin/marketplace.json` — the marketplace manifest for this repo

Load your working copy in a live session:

```bash
claude --plugin-dir ./signetry
```

### Exercising a change without an interactive session

`demos/try-guard.sh` drives the plugin's **real** `PreToolUse` hook with the exact
tool-call JSON Claude Code sends for `Edit` / `Write` / `Bash`, against a throwaway
repo with a sample `.signetry/admission.yaml`. It is the same code path a live
session hits, so it is the fastest way to check that enforcement still works:

```bash
bash demos/try-guard.sh
```

It needs `bash`, `git`, and a **Python >= 3.11** on `PATH` (the hook self-provisions
`signetry-core` into a plugin-local venv on first run — a stock macOS `python3` is
often 3.9 and is deliberately skipped).

### Before you open the PR

There is no test suite and no lint workflow in this repo, so check the two things
CI cannot check for you:

```bash
bash -n signetry/hooks/*.sh signetry/scripts/*.sh demos/try-guard.sh   # shell syntax
python3 -m json.tool signetry/.claude-plugin/plugin.json >/dev/null    # each JSON manifest parses
```

The shell files carry `# shellcheck` directives (`source=`, `disable=SC2086`), so
if you have [shellcheck](https://www.shellcheck.net/) installed, run it too and
keep it clean:

```bash
shellcheck signetry/hooks/*.sh signetry/scripts/*.sh demos/try-guard.sh
```

Two workflows run on every PR here:

- **CLA** (`.github/workflows/cla.yml`) — the signature gate described above.
- **Reviewer** (`.github/workflows/reviewer.yml` + `reviewer-comment.yml`) — an
  advisory `signetry-reviewer` pass that posts one recommendation comment. It is
  advisory only: it never merges and never fails the PR.

### Where a change belongs

- **This repo** — the hooks, the hook matchers and timeouts, the Python/venv
  resolution in `signetry-lib.sh`, the MCP launcher, the `/signetry:admit` skill
  text, the manifests, and the docs.
- **[`Signetry/core`](https://github.com/Signetry/core)** — the governance logic
  itself: contract evaluation, `signetry guard`, the independent verifier, earned
  authority, and receipt signing. If a decision is wrong — something was blocked
  that should not have been, or waved through that should not have been — the bug
  is almost certainly there, not here. This plugin never reimplements policy; it
  pins `signetry-core`.

### Things to keep in mind

- **The guard fails open, and that is deliberate** — it must never break a
  session. But "installed" must never be mistaken for "protected": when the guard
  cannot resolve a runner, the `SessionStart` hook prints a loud `INACTIVE` notice
  saying nothing is being enforced. If you touch the resolver, keep both halves of
  that contract. Admission (`/signetry:admit`) fails **closed**.
- `auto_merge` is always false. Signetry governs the agent; a human merges.
- The hooks read untrusted tool-call JSON from stdin. Keep it as data — never
  `eval` it, never interpolate it into a command line.
- `signetry-core` requires Python >= 3.11. Do not weaken the version check in
  `signetry-lib.sh` to make a machine with an older `python3` "work".
- Keep the `signetry-core` pin (`@v0.7.0`) consistent everywhere it appears —
  `signetry/hooks/signetry-lib.sh` (the venv install), `signetry-session-start.sh`,
  `signetry/scripts/signetry-mcp.sh`, `signetry/skills/admit/SKILL.md`, and
  `README.md`. `grep -rn v0.7.0 .` finds them all.
- Update [`CHANGELOG.md`](CHANGELOG.md) under `## [Unreleased]` for anything a
  user would notice.

Found a vulnerability? Do not open a public issue — use
[private reporting](https://github.com/Signetry/claude-code/security/advisories/new).
See [SECURITY.md](SECURITY.md).

By participating you agree to the [Code of Conduct](CODE_OF_CONDUCT.md).

## Credit

Contributors are **acknowledged** in [CONTRIBUTORS.md](CONTRIBUTORS.md), the Git
history, and release notes. See the "Recognition of Contributors" clause in
[CLA.md](CLA.md).
