# claude-workflow

<!-- Workflow template version: 1.1.0 -->

A Claude Code skill that eliminates context drift, silent design decisions,
and non-resumable sessions — by enforcing a document-driven pipeline where
files are the source of truth, not chat history.

---

## Quick start

```bash
# 1. Copy into your project
cp -r claude-workflow/.claude your-project/
cp -r claude-workflow/assets your-project/
cp    claude-workflow/CLAUDE.md your-project/
mkdir -p your-project/docs/plans your-project/docs/debug

# 2. Fill in CLAUDE.md — project name, tech stack, lint/test commands
```

In Claude Code:

```
Session type: FEATURE
/workflow
```

See [references/onboarding.md](references/onboarding.md) for the full setup guide.

---

## What's included

```
SKILL.md            ← skill entry point and overview
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
assets/
  plans/
    _research.md    ← research artifact template
    _plan.md        ← implementation plan template
    _todo.md        ← task breakdown template
  debug/
    _finding.md     ← debug finding template
references/
  overview.md       ← design rationale and concepts
  onboarding.md     ← step-by-step setup guide
evals/
  trigger-tests.md        ← when should the skill load
  functional-tests.md     ← stage-by-stage behavioral tests
  validation-checklist.md ← pre-release compliance checklist
CLAUDE.md           ← fill in for your project
```

---

## Documentation

- [Overview](references/overview.md) — design principles and concepts
- [Onboarding](references/onboarding.md) — installation and first session

---

## License

MIT
