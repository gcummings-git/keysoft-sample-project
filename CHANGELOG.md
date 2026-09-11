# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [v1.2.0] - 2026-09-11

### Added

- Order service package with order models, an in-memory store, business logic, HTTP-style request handlers, pytest tests, project config, README, task list, license and `.gitignore` (778d2b6, 349b458)
- Dev container configuration for Codespaces with a Python virtualenv and Claude Code pre-installed (a29d9a5)
- `/changelog` Claude Code skill that generates a Keep a Changelog section in `CHANGELOG.md` from git history (7221ae1)

### Changed

- Simplified module docstrings and the README project description, removing exercise-specific notes and inline "drift" comments; no behavior change (c8d5092)

### Removed

- Removed the GitHub Actions workflow that ran a Claude Code review on pull requests; it was added and removed within this release, so the net effect is none (fb39244, ed7316c)
