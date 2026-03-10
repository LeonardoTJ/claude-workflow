# Onboarding: Installing the Workflow in a New Project

---

## Prerequisites

- Claude Code CLI installed
- A git repository for your project
- Basic familiarity with Claude Code slash commands

---

## Step 1 — Copy the template files

From the root of your project:

```bash
# Copy the workflow configuration into your project
cp -r path/to/claude-workflow/.claude ./
cp -r path/to/claude-workflow/templates ./
cp    path/to/claude-workflow/CLAUDE.md ./
```

Create the docs scaffold:

```bash
mkdir -p docs/plans docs/debug
touch docs/plans/.gitkeep docs/debug/.gitkeep
```

Commit everything:

```bash
git add .claude/ templates/ docs/ CLAUDE.md
git commit -m "Add Claude Code workflow templates"
```

---

## Step 2 — Fill in CLAUDE.md

Open `CLAUDE.md` and replace all placeholder sections:

| Placeholder | What to write |
|-------------|---------------|
| `<Project Name>` | Your project name |
| Codebase Overview | One paragraph describing what the project is and what problem it solves |
| Tech Stack | Languages, frameworks, key dependencies, runtime versions |
| Linting command | e.g. `npm run lint` or `ruff check .` |
| Type checking command | e.g. `npm run typecheck` or `mypy src/` |
| Tests command | e.g. `npm test` or `pytest` |
| Branch naming | e.g. `feat/<slug>`, `fix/<slug>` |

The Implementer and Tester sub-agents read the commands from `CLAUDE.md` directly.
If these are blank, they cannot satisfy their own hard constraints.

---

## Step 3 — Verify the install

Open Claude Code in your project. Run:

```
/workflow
```

Claude should respond by asking for a session type and plan directory — not
by jumping straight into code. If it does, the skill loaded correctly.

---

## Step 4 — Run your first session

**Starting a new feature:**

```
Session type: FEATURE
/workflow
```

Claude will ask you to name the task. It will create `docs/plans/001-<slug>/`
and begin Stage 1 (Research).

**Resuming an interrupted session:**

```
Session type: FEATURE
Active plan: docs/plans/001-stripe-integration/
/workflow
```

Claude will read `plan.md` and `todo.md`, determine the current stage,
and continue from the first unchecked task.

---

## Step 5 — The annotation loop (Stage 3)

This is the most important step. When Claude writes `plan.md` and sets it to
`PENDING REVIEW`, open the file and annotate it directly:

```markdown
> NOTE: Do not use the deprecated Charges API — PaymentIntents only.
> NOTE: The webhook secret must come from env, not the config file.
> NOTE: Reduce scope — skip the refund endpoint for now.
```

Keep iterating ("don't implement yet") until the plan reflects exactly what
you want. Then change the status line to `APPROVED`. Only then tell Claude
to proceed to the todo stage.

You always decide what gets built. The plan is the contract.

---

## Reference: Session Bootstrap

Begin every non-trivial session with this pattern:

```
Session type: FEATURE | REFACTOR | DEBUG | ORCHESTRATE
Active plan: docs/plans/<NNN>-<slug>/   ← omit if starting fresh
/workflow   (or /debug, /refactor)
```

---

## Reference: Skill Summary

| Skill | Purpose |
|-------|---------|
| `/workflow` | Full planning protocol: research → plan → annotate → todo → implement → feedback |
| `/debug` | Focused bug investigation with a structured finding document |
| `/refactor` | Structural improvement without behavior change; includes regression verification |

---

## Troubleshooting

**Claude starts writing code immediately instead of researching.**
Check that `CLAUDE.md` contains the Working Protocol section. If it was
overwritten, restore it from the template.

**Claude can't find the template files.**
Verify `templates/plans/_research.md`, `_plan.md`, and `_todo.md` exist.
Sub-agents reference these paths directly.

**The session was interrupted and context was lost.**
Run the session bootstrap with the active plan directory. Claude reads all
state from `plan.md` and `todo.md` — conversation history is not needed.

**A sub-agent modified files outside its declared scope.**
The orchestrator should catch this via `git diff`. If it didn't, revert with
`git checkout -- <undeclared files>` and re-delegate with the scope constraint
added explicitly to the sub-agent's context.
