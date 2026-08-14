# Changelog — signetry-claude-code

Follows [Keep a Changelog](https://keepachangelog.com/) / [SemVer](https://semver.org/).

## [Unreleased]

### Changed

- Signetry naming throughout: the CLI is `signetry`; env vars use the
  `SIGNETRY_*` prefix; the config path is `.signetry/`; Python imports use
  `signetry_core`; and shell functions use the `signetry_*` prefix.
- The `signetry/` plugin asset directory (hooks, skills, scripts) and all
  references to it use the Signetry name.
- Pinned `signetry-core[...] @ git+https://github.com/Signetry/core@v0.6.0`
  and the advisory reviewer to
  `signetry-reviewer @ git+https://github.com/Signetry/reviewer@v0.6.0`.

## [0.3.0] — 2026-07-26

### Added

- Split out of the `signetry-plugins` monorepo into a dedicated repository under the
  [Signetry umbrella](https://github.com/Signetry/signetry), per the platform
  architecture (one repo per integration).
- Pins `signetry-core>=0.3.0` (capability graph, plan binding, masked verifier,
  G1/G2/G3 gates, extension admission).
