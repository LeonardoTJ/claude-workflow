# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

---

## [1.1.0] — 2026-03-10

Aligned with Anthropic's official skill standard (custom commands merged into skills).

### Added

- `SKILL.md` — root skill file with YAML frontmatter; enables proper skill
  discovery and progressive disclosure
- `references/overview.md` — design rationale, principles, and artifact layout
  (moved from `docs/overview.md`)
- `references/onboarding.md` — installation and first session guide
  (moved from `docs/onboarding.md`)
- `assets/plans/_research.md` — research artifact template (moved from `templates/`)
- `assets/plans/_plan.md` — implementation plan template (moved from `templates/`)
- `assets/plans/_todo.md` — task breakdown template (moved from `templates/`)
- `assets/debug/_finding.md` — debug finding template (moved from `templates/`)
- `evals/trigger-tests.md` — trigger test cases for the skill description field
- `evals/functional-tests.md` — Given/When/Then behavioral tests per stage
- `evals/validation-checklist.md` — pre-release structural compliance checklist

### Changed

- `templates/` renamed to `assets/` — matches Anthropic skill standard nomenclature
- `docs/` static files moved to `references/` — third-level progressive disclosure
- `README.md` refocused as GitHub/human-facing only; runtime content moved to
  `SKILL.md` and `references/`
- All `templates/` path references updated to `assets/` across commands and profiles

### Removed

- `templates/` directory (replaced by `assets/`)
- `docs/overview.md` (moved to `references/overview.md`)
- `docs/onboarding.md` (moved to `references/onboarding.md`)

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
- `assets/plans/_research.md` — research artifact template
- `assets/plans/_plan.md` — implementation plan template
- `assets/plans/_todo.md` — task breakdown and progress tracker template
- `assets/debug/_finding.md` — debug finding template
- `references/overview.md` — design rationale, principles, and artifact layout
- `references/onboarding.md` — step-by-step installation and first session guide
- `README.md` — project overview and quick start
