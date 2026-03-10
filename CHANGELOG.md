# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

---

## [1.0.0] — 2026-03-09

### Added

- `CLAUDE.md` — minimal project-level instructions, always loaded into context
- `.claude/commands/workflow.md` — full planning protocol skill (`/workflow`)
- `.claude/commands/debug.md` — bug investigation protocol skill (`/debug`)
- `.claude/commands/refactor.md` — refactor protocol skill (`/refactor`)
- `.claude/profiles/orchestrator.md` — coordinates sub-agents, owns `todo.md`
- `.claude/profiles/researcher.md` — reads code, writes findings
- `.claude/profiles/architect.md` — translates findings into implementation plans
- `.claude/profiles/implementer.md` — executes declared phases
- `.claude/profiles/tester.md` — writes and runs tests, reports gaps
- `.claude/profiles/documenter.md` — produces consolidated reports and findings
- `templates/plans/_research.md` — research artifact template
- `templates/plans/_plan.md` — implementation plan template
- `templates/plans/_todo.md` — task breakdown and progress tracker template
- `templates/debug/_finding.md` — debug finding template
- `docs/overview.md` — design rationale, principles, and artifact layout
- `docs/onboarding.md` — step-by-step installation and first session guide
- `README.md` — project overview and quick start
