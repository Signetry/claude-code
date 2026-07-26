# Changelog — umbra-claude-code

Follows [Keep a Changelog](https://keepachangelog.com/) / [SemVer](https://semver.org/).

## [0.3.0] — 2026-07-26

### Added

- Split out of the `umbra-plugins` monorepo into a dedicated repository under the
  [Umbra umbrella](https://github.com/bkd-dotcom/umbra-umbrella), per the platform
  architecture (one repo per integration).
- Pins `umbra-core>=0.3.0` (capability graph, plan binding, masked verifier,
  G1/G2/G3 gates, extension admission).
