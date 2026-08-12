# Changelog — umbra-claude-code

Follows [Keep a Changelog](https://keepachangelog.com/) / [SemVer](https://semver.org/).

## [Unreleased]

### Changed

- Rebranded the platform from Umbra to Signetry with no backward-compatibility
  fallbacks: the `umbra` CLI is now `signetry`; env vars `UMBRA_*` are now
  `SIGNETRY_*`; the config path `.umbra/` is now `.signetry/`; Python imports
  `umbra_core` are now `signetry_core`; and shell functions `umbra_*` are now
  `signetry_*`.
- Renamed the `umbra/` plugin asset directory (hooks, skills, scripts) to
  `signetry/` and updated all references.
- Repinned `signetry-core[...] @ git+https://github.com/Signetry/core@v0.6.0`
  (dropping the old `>=` version specs) and the advisory reviewer to
  `signetry-reviewer @ git+https://github.com/Signetry/reviewer@v0.6.0`.

## [0.3.0] — 2026-07-26

### Added

- Split out of the `umbra-plugins` monorepo into a dedicated repository under the
  [Umbra umbrella](https://github.com/Signetry/signetry), per the platform
  architecture (one repo per integration).
- Pins `umbra-core>=0.3.0` (capability graph, plan binding, masked verifier,
  G1/G2/G3 gates, extension admission).
