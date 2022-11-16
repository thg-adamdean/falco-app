# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- Added `VerticalPodAutoscaler` to `falco`.

## [0.4.0] - 2022-11-09

### Changed

- **Note:** Due to breaking changes in the official chart, it is not possible to cleanly replace an existing Falco DaemonSet from a previous version with this new version. As a result, when updating to this version, there will be a short Falco downtime while the existing DaemonSet is deleted and replaced.
- Update `falco` to upstream version 0.33.0 / chart version 2.2.0.
- Update `falco-exporter` to upstream version 0.8.0 / chart version 0.9.1.
- Update `falcosidekick` to upstream version 2.26.0 / chart version 0.5.9.

## [0.3.2] - 2022-03-25

### Changed

- Use `ebpf` driver instead of kernel module by default.

## [0.3.1] - 2022-03-10

### Changed

- Set `priorityClass` of `giantswarm-critical` for Falco DaemonSet.

## [0.3.0] - 2022-02-28

### Changed

- Update to upstream falco 1.17.2/0.31.0.
- Update to upstream falco-exporter 0.8.0/0.7.0.

## [0.2.0] - 2021-12-17

### Changed

- Update to upstream charts: Falco 1.16.2/0.30.0, exporter 0.6.3/0.6.0, sidekick 0.4.4/2.24.0.

## [0.1.2] - 2021-10-15

### Changed

- App icon

## [0.1.1] - 2021-08-26

### Added

- Push `falco-app` to `giantswarm` catalog.

## [0.1.0] - 2021-07-22

### Added

- Set `requests` and `limits` for falco-exporter.

### Changed

- Update charts to latest upstream versions.
- Push `falco-app` to provider collections (except KVM) when tagged.
- Use Giant Swarm-managed images.

[Unreleased]: https://github.com/giantswarm/falco-app/compare/v0.4.0...HEAD
[0.4.0]: https://github.com/giantswarm/falco-app/compare/v0.3.2...v0.4.0
[0.3.2]: https://github.com/giantswarm/falco-app/compare/v0.3.1...v0.3.2
[0.3.1]: https://github.com/giantswarm/falco-app/compare/v0.3.0...v0.3.1
[0.3.0]: https://github.com/giantswarm/falco-app/compare/v0.2.0...v0.3.0
[0.2.0]: https://github.com/giantswarm/falco-app/compare/v0.1.2...v0.2.0
[0.1.2]: https://github.com/giantswarm/falco-app/compare/v0.1.1...v0.1.2
[0.1.1]: https://github.com/giantswarm/falco-app/compare/v0.1.0...v0.1.1
[0.1.0]: https://github.com/giantswarm/falco-app/releases/tag/v0.1.0
