---
name: workflow
description: >
  Structured, document-driven engineering workflow for Claude Code. Enforces a
  six-stage pipeline (Research → Plan → Annotate → Todo → Implement → Feedback)
  before writing any code. All state lives in files — sessions are resumable
  across context resets and machines. Supports sub-agent orchestration with
  specialized roles. Use when starting a feature, refactor, or debug session,
  or when user says /workflow, /debug, /refactor, "start a feature", "plan this
  change", "investigate this bug", "I need a plan", "new session", "resume
  session", "FEATURE", "REFACTOR", "DEBUG", or "ORCHESTRATE".
license: MIT
compatibility: Optimized for Claude Code. Requires .claude/commands/ and
  .claude/profiles/. Runtime artifacts are written to docs/plans/ and docs/debug/.
metadata:
  author: leonardotj
  version: 1.1.0
  category: workflow-automation
  tags: [planning, document-driven, multi-agent, engineering]
---

# Workflow

## What This Skill Does

Unstructured AI sessions drift. Design decisions get made silently, findings
are buried in scroll-back, and a crashed context means starting from zero.
This skill enforces a document-first pipeline where every significant decision
is written to a file before code is touched. The files — not chat history —
are the source of truth. Any session can be resumed on any machine by reading
`plan.md` and `todo.md`.

## Session Bootstrap

Begin every non-trivial session with:

```
Session type: FEATURE | REFACTOR | DEBUG | ORCHESTRATE
Active plan:  docs/plans/<NNN>-<slug>/   ← omit if starting fresh
/workflow   (or /debug, /refactor)
```

## Available Skills

| Skill | Invoke | Purpose |
|-------|--------|---------|
| Workflow | `/workflow` | Feature planning: Research → Plan → Annotate → Todo → Implement → Feedback |
| Debug | `/debug` | Bug investigation: Reproduce → Hypothesize → Verify → Fix → Document |
| Refactor | `/refactor` | Structural improvement without behavior change |

Full stage-by-stage protocols are in `.claude/commands/`.

## Artifact Layout

At runtime, Claude writes session artifacts into:

```
docs/
├── plans/
│   └── <NNN>-<slug>/
│       ├── research.md    ← Stage 1 output
│       ├── plan.md        ← Stage 2 output  [PENDING REVIEW → APPROVED → COMPLETE]
│       ├── todo.md        ← Stage 4 output  [live progress tracker]
│       └── report.md      ← Stage 6 output  [session summary]
└── debug/
    └── YYYY-MM-DD-<slug>.md   ← /debug finding
```

Templates for each artifact are in `assets/plans/` and `assets/debug/`.

## Sub-Agent Profiles

For `ORCHESTRATE` sessions, the orchestrator spawns specialized sub-agents.
Each profile is in `.claude/profiles/` and is passed by the orchestrator —
users do not invoke profiles directly.

| Profile | Model | Role |
|---------|-------|------|
| orchestrator | claude-opus-4-6 | Coordinates agents, sole writer of todo.md |
| researcher | claude-opus-4-6 | Reads files deeply, documents findings only |
| architect | claude-opus-4-6 | Translates research into an annotatable plan |
| implementer | claude-sonnet-4-6 | Executes one declared phase per invocation |
| tester | claude-sonnet-4-6 | Writes and runs tests, reports failures |
| documenter | claude-haiku-4-5 | Produces session reports and debug findings |

## Design Principles

- **Document-first** — no code is written until `plan.md` status is `APPROVED`
- **Human autonomy** — the human annotates `plan.md` directly with `> NOTE:`
  comments; annotations are accepted without debate
- **Strict scope** — each agent receives only its declared files; undeclared
  changes are flagged and reverted
- **Revert-before-retry** — on failure, `git checkout -- <files>` before
  re-delegating; no forward workarounds

## References

- `references/overview.md` — design rationale, session types, and concepts
- `references/onboarding.md` — installation guide and first session walkthrough
- `assets/plans/` — research, plan, and todo artifact templates
- `assets/debug/` — debug finding template
