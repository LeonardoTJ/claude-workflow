# claude-workflow

<!-- Workflow template version: 1.0.0 -->

A set of Claude Code configuration templates that impose a structured,
document-driven process on AI-assisted engineering sessions — eliminating
context drift, silent design decisions, and non-resumable sessions.

---

## How it works

Every non-trivial task goes through six stages before any code is written:

```
Research → Plan → Annotate → Todo → Implement → Feedback
```

Each stage produces a file. The files are the source of truth — not chat
history. Sessions can be interrupted and resumed on any machine.

The human annotates the plan directly before implementation begins.
You always decide what gets built.

---

## Quick start

```bash
# 1. Copy into your project
cp -r claude-workflow/.claude your-project/
cp -r claude-workflow/templates your-project/
cp    claude-workflow/CLAUDE.md your-project/
mkdir -p your-project/docs/plans your-project/docs/debug

# 2. Fill in CLAUDE.md — project name, tech stack, lint/test commands

# 3. Start a session in Claude Code
```

In Claude Code:

```
Session type: FEATURE
/workflow
```

See [docs/onboarding.md](docs/onboarding.md) for the full setup guide.

---

## What's included

```
.claude/
  commands/
    workflow.md     ← /workflow  — full planning protocol
    debug.md        ← /debug     — bug investigation protocol
    refactor.md     ← /refactor  — structural improvement protocol
  profiles/
    orchestrator.md   ← coordinates sub-agents, owns todo.md
    researcher.md     ← reads code, writes findings (Opus)
    architect.md      ← translates findings into plans (Opus)
    implementer.md    ← executes one phase at a time (Sonnet)
    tester.md         ← writes and runs tests (Sonnet)
    documenter.md     ← produces reports and findings (Haiku)
templates/
  plans/
    _research.md    ← research artifact template
    _plan.md        ← implementation plan template
    _todo.md        ← task breakdown template
  debug/
    _finding.md     ← debug finding template
docs/
  overview.md       ← design rationale and concepts
  onboarding.md     ← step-by-step setup guide
CLAUDE.md           ← fill in for your project
```

---

## Documentation

- [Overview](docs/overview.md) — design principles and concepts
- [Onboarding](docs/onboarding.md) — installation and first session

---

## License

MIT
