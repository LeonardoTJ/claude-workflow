# Skill: Refactor Session

You are now in a focused refactor session. The goal is to improve structure,
readability, or performance without changing observable behavior.
All refactor work follows the same document-driven process as features.

---

## On Invocation

When this skill fires:

1. **Confirm the refactor slug**: a short kebab-case label (e.g. `extract-payment-service`)
2. **Identify or create the plan directory**: `docs/plans/<NNN>-<slug>/`
3. **State the refactor goal in one sentence** and confirm it with the human before proceeding

---

## Stage 1 — Scope

Before reading any code, define and write to `docs/plans/<NNN>-<slug>/research.md`:

- **What** is being refactored (files, modules, patterns)
- **Why** — the specific problem being addressed: duplication, coupling, complexity, performance
- **What is not changing** — observable behavior, public interfaces, data contracts, test expectations

Do not begin investigation until the scope is agreed with the human.

---

## Stage 2 — Research

Read all in-scope files **deeply and in great detail** — do not skim.

Document in `research.md`:
- Current structure: module boundaries, data flow, coupling points
- Every caller of any interface that may change
- Existing test coverage of affected code — this is the regression net
- Risks: anything that could break silently if changed
- **Open Questions** for anything that cannot be determined from the code alone

---

## Stage 3 — Plan

Write `docs/plans/<NNN>-<slug>/plan.md` using `templates/plans/_plan.md`.

Each phase must include:
- Exact files affected
- Before and after function/type signatures for any interface that changes
- Migration strategy for all callers of changed interfaces — callers are in scope if interfaces change
- Explicit statement of what behavior is preserved (not changed)
- Set status to `PENDING REVIEW`

Do not begin implementation.

---

## Stage 4 — Annotate (Human Stage)

Same rules as the feature workflow:

- Human annotates `plan.md` with `> NOTE:` prefixed comments
- Human may reject approaches, constrain scope, or provide examples
- Do not proceed until the human removes `PENDING REVIEW` and replaces it with `APPROVED`
- Accept all annotations without debate

---

## Stage 5 — Todo

Generate `docs/plans/<NNN>-<slug>/todo.md` from the approved plan.

Always include a final **Regression Check** phase:
- [ ] Run lint
- [ ] Run type check
- [ ] Run full test suite
- [ ] `git diff main` — verify no undeclared files changed

---

## Stage 6 — Implement

- Same rules as feature implementation: work through tasks in order, do not cherry-pick
- After **each phase**: run the test suite immediately — a refactor that breaks tests is not complete
- If a test fails: revert the phase with `git checkout -- <files>` before retrying — do not push forward
- Do not fix failing tests by modifying the tests unless the test was provably wrong before the refactor
- Mark tasks complete in `todo.md` as each one is finished

---

## Stage 7 — Verify

When all tasks are complete:

1. Run the full test suite
2. Run lint and type checking
3. Run `git diff main` — every changed file must appear in the plan's declared scope
4. Confirm no observable behavior changed: same inputs produce the same outputs

Present the verification results to the human before declaring the session complete.

---

## Hard Constraints

- A refactor that changes behavior is no longer a refactor — stop and convert to a FEATURE session
- Do not add new features during a refactor, even small, obvious ones
- Do not fix bugs encountered in passing — log them as out-of-scope observations in `todo.md`
- Tests are the contract: if all tests pass before and after, the refactor is safe
- If the refactor reveals that no tests cover the affected code, surface that to the human before proceeding
