# Functional Tests: workflow

Verify each skill produces correct outputs at each stage. Use Given/When/Then
format. Run manually in Claude Code.

---

## /workflow

### Test 1 — Stage 1: Research output

```
Given: A new plan directory docs/plans/001-test-feature/ does not exist
When:  Session type FEATURE is declared and /workflow is invoked
Then:  - Claude asks for a task name before reading any code
       - docs/plans/001-test-feature/research.md is created
       - research.md follows the structure of assets/plans/_research.md
         (sections: Question, Files Examined, Dependency Map, Current Behavior,
          Edge Cases and Risks, Open Questions)
       - No plan.md or todo.md is created yet
       - No code is written
```

### Test 2 — Stage 2: Plan output

```
Given: docs/plans/001-test-feature/research.md exists
When:  Plan generation is requested
Then:  - Claude reads research.md before writing anything
       - docs/plans/001-test-feature/plan.md is created
       - plan.md status is "PENDING REVIEW" at the top
       - plan.md follows the structure of assets/plans/_plan.md
         (sections: Objective, Assumptions, Affected Files, Phases, Decision Log)
       - Each phase includes: file paths, before/after signatures, rationale,
         alternatives considered
       - No code is written
       - No todo.md is created yet
```

### Test 3 — Stage 3 gate: Annotation enforcement

```
Given: plan.md exists with status "PENDING REVIEW"
When:  User requests "start implementing" or "begin Stage 5"
Then:  - Claude refuses to implement
       - Claude explains the annotation requirement
       - Claude instructs user to change status to "APPROVED"
       - No files are modified
```

### Test 4 — Stage 4: Todo output

```
Given: plan.md status is "APPROVED"
When:  Todo generation is requested
Then:  - Claude reads plan.md before writing
       - docs/plans/001-test-feature/todo.md is created
       - todo.md uses GFM checkboxes: - [ ] task description
       - Tasks are grouped into named phases matching plan.md
       - A "Regression Check" phase is included at the end
       - Every task maps to a specific file change or shell command
```

### Test 5 — Stage 5: Implementation progress tracking

```
Given: todo.md exists with unchecked tasks
When:  Implementation begins
Then:  - Claude reads plan.md and todo.md before writing any code
       - Tasks are completed in order (no cherry-picking)
       - After each task: - [x] is marked in todo.md
       - Lint and type check run after each phase (not just at the end)
       - If a blocker is encountered: BLOCKED is added to todo.md before stopping
```

### Test 6 — Resume: Continue from interrupted session

```
Given: plan.md status is "APPROVED"
        todo.md has some tasks checked [x] and some unchecked [ ]
When:  Session type FEATURE is declared with active plan directory
Then:  - Claude reads plan.md for context
       - Claude reads todo.md for progress
       - Claude identifies and states the first unchecked task
       - Execution continues from that task
       - Previously completed tasks are not repeated
```

---

## /debug

### Test 7 — Finding file created before code is read

```
Given: User describes a bug: "NullPointerException in checkout when cart is empty"
When:  /debug is invoked
Then:  - Claude creates docs/debug/YYYY-MM-DD-<slug>.md immediately
       - The finding file follows assets/debug/_finding.md structure
       - "Observed Behavior" and "Expected Behavior" are filled in
         from the user's description before any code is read
       - Claude states the finding file path before proceeding
       - No code files are read yet at this point
```

### Test 8 — Root cause confirmed before fix

```
Given: Investigation is underway (Stages 2-3 complete)
When:  User asks "just fix it quickly"
Then:  - Claude refuses to fix before Stage 4 (Verify) is complete
       - Claude explains that hypothesis must be confirmed first
       - No code files are modified
```

---

## /refactor

### Test 9 — Scope defined before code is read

```
Given: User says "refactor the payment service"
When:  /refactor is invoked
Then:  - Claude asks to define: WHAT, WHY, and WHAT IS NOT CHANGING
       - Claude does not read any code until scope is agreed
       - Scope is written to research.md before investigation begins
```

### Test 10 — Stage 4 annotation gate

```
Given: refactor plan.md exists with status "PENDING REVIEW"
When:  User requests "start refactoring"
Then:  - Claude refuses to implement
       - Claude waits for status "APPROVED"
       - Same behavior as /workflow Stage 3 gate
```

### Test 11 — Behavior change detection

```
Given: Refactor is underway
When:  A planned change would alter observable behavior
        (e.g., function signature change that affects callers not in scope)
Then:  - Claude stops and flags the behavior change
       - Claude suggests converting to a FEATURE session
       - No implementation proceeds without human decision
```

---

## Orchestrator (ORCHESTRATE session)

### Test 12 — Profile selection by task type

```
Given: An APPROVED plan with 3 phases:
        Phase 1: code investigation
        Phase 2: database migration
        Phase 3: session report
When:  Orchestrator runs
Then:  - Phase 1 → researcher profile (claude-opus-4-6)
       - Phase 2 → implementer profile (claude-sonnet-4-6)
       - Phase 3 → documenter profile (claude-haiku-4-5)
       - Each sub-agent receives ONLY its phase's declared files
       - Full plan.md is NOT passed to sub-agents
```

### Test 13 — Scope validation before marking complete

```
Given: Implementer sub-agent finishes a phase
When:  Orchestrator validates output
Then:  - git diff is run on all modified files
       - Any file not in the phase's declared scope is flagged
       - Task is NOT marked [x] until validation passes
       - If undeclared files exist: revert + re-delegate before escalating
```
