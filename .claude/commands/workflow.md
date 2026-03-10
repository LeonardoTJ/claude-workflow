# Skill: Structured Planning Workflow

You are now operating under the full document-based planning protocol.
All plan artifacts are the source of truth — not chat history.

---

## On Invocation

When this skill fires, before doing any stage work:

1. **Confirm session type** if not already declared: `FEATURE | REFACTOR | DEBUG | ORCHESTRATE`
2. **Identify or create the plan directory**: `docs/plans/<NNN>-<slug>/`
   - Pick the next available NNN by checking existing directories (zero-padded: 001, 002, …)
   - slug is a short kebab-case label for the task (e.g. `stripe-integration`)
3. **Determine starting stage** by checking what exists in the plan directory:
   - No `plan.md` → begin at **Stage 1 (Research)**
   - `plan.md` status `PENDING REVIEW` → resume at **Stage 3 (Annotate)**
   - `plan.md` status `APPROVED`, `todo.md` has unchecked tasks → resume at **Stage 5 (Implement)**
   - All tasks checked → proceed to **Stage 6 (Feedback)**
4. **State your starting stage and active plan directory** before proceeding.

The active plan directory is `docs/plans/<NNN>-<slug>/` unless stated otherwise.

---

## Stage 1 — Research

When asked to research a topic or feature area:

- Read all relevant source files thoroughly. Do not skim. Use keywords like
  "deeply" and "in detail" to calibrate reading depth.
- Write findings to `docs/plans/<NNN>-<slug>/research.md` using the template
  at `templates/plans/_research.md`
- Include: every file examined, key functions and modules, dependencies,
  current behavior, edge cases, and anything surprising or risky
- Close with an **Open Questions** section for anything that could not be
  determined from the available code
- Do not propose a solution yet

---

## Stage 2 — Plan

When asked to generate an implementation plan:

- Read `research.md` for this task before writing anything
- Write the plan to `docs/plans/<NNN>-<slug>/plan.md` using the template
  at `templates/plans/_plan.md`
- Each change must include: exact file path, function signatures, code
  snippets, rationale, tradeoffs considered, and alternatives rejected with reasons
- Flag all assumptions explicitly
- Set status to `PENDING REVIEW` at the top of the file
- Do not begin implementation

---

## Stage 3 — Annotate (Human Stage)

The human annotates `plan.md` directly. Annotations use the prefix `> NOTE:`.

Do not proceed to implementation until the human:
- Removes `PENDING REVIEW`
- Replaces it with `APPROVED`

During annotation the human may reject approaches, add constraints, supply
code examples, links, or reduce scope. Accept all annotations without debate.

---

## Stage 4 — Todo

When asked to generate a todo list:

- Read the approved `plan.md`
- Write the task breakdown to `docs/plans/<NNN>-<slug>/todo.md` using the
  template at `templates/plans/_todo.md`
- Use GFM checkboxes: `- [ ] description`
- Group tasks into named phases (e.g. Phase 1: Data layer, Phase 2: API)
- Every task must map to a specific file change or shell command
- Tasks must be granular enough that each one is completable in a single
  focused step

---

## Stage 5 — Implement

When told to begin implementation:

- Read `plan.md` and `todo.md` before writing any code
- Work through tasks in order. Do not cherry-pick
- After each completed task, mark it in `todo.md`: `- [x] description`
- `todo.md` is the live progress tracker — keep it accurate at all times
- Run linting and type checking after each phase, not only at the end
- If a blocker is encountered:
  1. Add `- [ ] BLOCKED: <description>` to `todo.md`
  2. Document the error inline below the blocked item
  3. Surface it to the human before continuing
  4. Do not attempt workarounds that fall outside the approved plan
- Do not stop until all tasks are marked complete or a blocker is raised

---

## Stage 6 — Feedback and Iteration

When corrections are requested after review:

- Small change: apply directly, referencing the specific file and function
- Significant change: stop, revert with `git revert` or `git checkout`, and
  re-scope the plan before re-implementing
- Append all non-obvious decisions made during implementation to a
  `## Decision Log` section at the bottom of `plan.md`
- Update `plan.md` status to `COMPLETE` when all tasks pass review

---

## Debug Interrupt Handling

If an error occurs during a FEATURE session:

1. Add `- [ ] BLOCKED: <one-line error summary>` to `todo.md`
2. Do not investigate inline
3. Notify the human — suggest opening a DEBUG session for the issue
4. Wait for the human to resolve the blocker before continuing

---

## Workflow Resume

To resume an interrupted session on any machine:

1. Declare session type and invoke this skill
2. Provide the active plan directory path
3. Read `plan.md` for context and `todo.md` for current progress
4. Continue from the first unchecked task
