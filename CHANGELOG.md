# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.0] - 2026-11-20

### Added

- Initial release of the `al` skill.
- Run a registered entry by name: `/al <name> [arguments]`.
- Register an entry: `/al <name>: <content>`.
- List the entries in effect: `/al`.
- `--global` flag to list, run or register entries in `~/.al.yaml` only.
- Entries are kept in `.al.yaml` in the project root and in `~/.al.yaml`;
  the project entry wins when the same name exists in both.
- An entry value is either instruction text or a path to a file or skill directory.

[Unreleased]: https://github.com/progrhyme/agent-skill-al/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/progrhyme/agent-skill-al/releases/tag/v0.1.0
