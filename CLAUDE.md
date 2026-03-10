# <Project Name>

<!-- Workflow template version: 1.0.0 — github.com/leonardotj/claude-workflow -->

## Codebase Overview
<!-- One paragraph: what this project is, what problem it solves -->

## Tech Stack
<!-- Languages, frameworks, key dependencies, runtime versions -->

## Conventions
- Edit existing files when fixing bugs; do not create new files for fixes
- Linting: `<command>`
- Type checking: `<command>`
- Tests: `<command>`
- Branch naming: `<convention>`

## Directory Layout
- `src/` — application source
- `docs/plans/<NNN>-<slug>/` — per-task planning artifacts (research, plan, todo)
- `docs/debug/` — debug findings
- `.claude/profiles/` — sub-agent profiles
- `.claude/commands/` — slash command skills

## Working Protocol
All non-trivial work uses the structured planning workflow defined in the
`/workflow` skill. Invoke it at the start of any feature, refactor, or
orchestrated session.

For quick one-off tasks (small fixes, questions, lookups) no skill is needed.

## Session Bootstrap
Begin every non-trivial session with:
1. Declare session type: FEATURE | REFACTOR | DEBUG | ORCHESTRATE
2. Invoke the appropriate skill: `/workflow`, `/debug`, or `/refactor`
3. State the active plan directory if resuming: `docs/plans/<NNN>-<slug>/`
