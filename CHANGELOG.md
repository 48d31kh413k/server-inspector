# Changelog

All notable changes to **server-inspector** are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

## [1.0.0] - 2026-03-20

### Added
- Initial public release of `server_info.sh`
- Sections: Host/OS, CPU, Memory, Disk, Network, Virtualization, Users & Security,
  Active Services, Top Processes, Docker
- `--output FILE` flag to save report to a file
- `--no-color` flag for plain-text output
- `--version` and `--help` flags
- Graceful fallbacks when optional commands (`lscpu`, `dmidecode`, `docker`, etc.) are absent
- Coloured, structured terminal output with section headers
- GitHub Actions CI workflow with ShellCheck linting
- MIT License
- Comprehensive README with usage examples and contribution guide

### Fixed
- Corrected `cu` → `cut` typo in OS name extraction
- Removed stray trailing `t` character at end of script

[Unreleased]: https://github.com/48d31kh413k/server-inspector/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/48d31kh413k/server-inspector/releases/tag/v1.0.0
