# Workflow Overview

A collection of Claude Code configuration templates that impose a structured,
document-driven process on AI-assisted engineering sessions.

---

## The Problem It Solves

Unstructured AI coding sessions tend to fail in a predictable pattern:
context grows until earlier findings are compressed away, the model makes
design decisions silently, there is no record of what was decided or why,
and the session cannot be resumed cleanly. When something goes wrong, there
is no artifact to review — only a long chat history.

This workflow replaces chat history as the source of truth with files.

---

## Core Idea

Every non-trivial task produces three artifacts before any code is written:

1. **`research.md`** — what the codebase actually does (not what it should do)
2. **`plan.md`** — how the change will be made, with rationale and tradeoffs
3. **`todo.md`** — a granular, phase-by-phase task list used as a live progress tracker

These files persist across sessions, survive context resets, and can be read
by any sub-agent spawned later without passing conversation history.

---

## Design Principles

**Document-first.** No code is written until `plan.md` is `APPROVED`. The
document is annotated by the human until it accurately represents what should
be built. This prevents silent design decisions and scope creep.

**Human autonomy at every gate.** The human annotates the plan directly with
`> NOTE:` comments — rejecting approaches, supplying constraints, trimming
scope. Claude cannot proceed past the annotation stage without explicit approval.
The human always decides what gets built and how.

**Strict scope boundaries.** Each sub-agent receives only the files it needs
for its declared phase. Undeclared file changes are flagged by the orchestrator
before marking any task complete.

**Revert before retry.** When something goes wrong during implementation,
the default action is `git checkout -- <files>` followed by a re-scoped
attempt — not a forward workaround that diverges from the plan.

**Cost-tiered models.** Heavy reasoning tasks (research, architecture) use
Opus. Execution tasks (implementation, testing) use Sonnet. Reporting uses
Haiku. This keeps token cost proportional to task complexity.

---

## Session Types

| Type | Skill | When to use |
|------|-------|-------------|
| `FEATURE` | `/workflow` | New functionality, significant changes |
| `REFACTOR` | `/refactor` | Structural improvement, no behavior change |
| `DEBUG` | `/debug` | Investigating and fixing a specific bug |
| `ORCHESTRATE` | `/workflow` | Multi-agent delegation of an approved plan |

---

## Artifact Layout

```
docs/
  plans/
    <NNN>-<slug>/
      research.md     ← Stage 1 output
      plan.md         ← Stage 2 output (PENDING REVIEW → APPROVED → COMPLETE)
      todo.md         ← Stage 4 output (live progress tracker)
      report.md       ← Stage 6 output (session summary)
  debug/
    YYYY-MM-DD-<slug>.md   ← debug findings
```

---

## Resuming a Session

Because all state is in files, any session can be resumed on any machine:

1. Declare session type and invoke the appropriate skill
2. Provide the active plan directory path
3. The skill reads `plan.md` for context and `todo.md` for progress
4. Work resumes from the first unchecked task
