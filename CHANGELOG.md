# Changelog — signetry-claude-code

Follows [Keep a Changelog](https://keepachangelog.com/) / [SemVer](https://semver.org/).

## [Unreleased]

### Changed — the project is now open source (Apache-2.0)

- Signetry moved to an **open-core** model. This repository is the integration
  surface, so it is now **Apache-2.0**: use it, fork it, ship it commercially, no
  strings. The engine ([`Signetry/core`](https://github.com/Signetry/core)) is
  source-available under BUSL-1.1 and converts to Apache-2.0 on 2030-08-31. See
  [LICENSING.md](https://github.com/Signetry/signetry/blob/main/LICENSING.md).
- `signetry/.claude-plugin/plugin.json` now declares `"license": "Apache-2.0"`,
  which **supersedes the `"Proprietary — All Rights Reserved"` value set by the
  entry below** in this same unreleased range.
- `README.md`, `CONTRIBUTING.md`, `CLA.md`, `CONTRIBUTORS.md`, and the CLA
  workflow's PR comment no longer describe the project as "All Rights Reserved" or
  "not open source".
- The **CLA is kept**. Apache-2.0 already grants contributors every right the old
  wording withheld; the CLA now exists for the relicensing rights that let a
  well-built adapter move into the BUSL-1.1 engine later without chasing down
  every past contributor.
- **The CLA's fallback licence grant is now non-exclusive.** It previously granted the
  Owner an *exclusive* licence where copyright assignment is not permitted by law, which
  would have stripped contributors of the right to use their own contribution — directly
  contradicting the rights the LICENSE grants everyone. The CLA text is now identical
  across all Signetry repositories (bar the engine/integration licence wording) so the
  legal terms cannot drift per-repo again. See [CLA.md](CLA.md) §2–3.

### Added

- Issue templates under `.github/ISSUE_TEMPLATE/` (bug report, feature request,
  and a config that routes vulnerabilities to private reporting and
  governance-logic bugs to `signetry-core`).

### Fixed — the plugin declared the wrong license

- `signetry/.claude-plugin/plugin.json` declared `"license": "MIT"` while the
  project is **All Rights Reserved**. Left as-is, the plugin directory arguably
  shipped under MIT terms — i.e. granted rights the rest of the project reserves.
  Now `"Proprietary — All Rights Reserved"`, matching `signetry-core`'s
  `pyproject.toml`.

### Changed

- Signetry naming throughout: the CLI is `signetry`; env vars use the
  `SIGNETRY_*` prefix; the config path is `.signetry/`; Python imports use
  `signetry_core`; and shell functions use the `signetry_*` prefix.
- The `signetry/` plugin asset directory (hooks, skills, scripts) and all
  references to it use the Signetry name.
- Pinned `signetry-core[...] @ git+https://github.com/Signetry/core@v0.6.0`
  and the advisory reviewer to
  `signetry-reviewer @ git+https://github.com/Signetry/reviewer@v0.1.2`.

## [0.3.0] — 2026-07-26

### Added

- Split out of the `signetry-plugins` monorepo into a dedicated repository under the
  [Signetry umbrella](https://github.com/Signetry/signetry), per the platform
  architecture (one repo per integration).
- Pins `signetry-core>=0.3.0` (capability graph, plan binding, masked verifier,
  G1/G2/G3 gates, extension admission).
