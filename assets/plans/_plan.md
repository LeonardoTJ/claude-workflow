# Plan: <slug>

**Status:** PENDING REVIEW
**Plan directory:** `docs/plans/<NNN>-<slug>/`
**Date:** YYYY-MM-DD

---

## Objective

<!-- One paragraph: what this plan builds or changes and why. -->

---

## Assumptions

<!-- List all assumptions explicitly. Never leave an assumption implicit. -->

-

---

## Affected Files

<!-- Every file that will be created or modified. No other files may be touched. -->

- `path/to/file` — reason

---

## Phases

### Phase 1: <name>

**Files:** `path/to/file`

**Changes:**

<!-- Describe the change. Include function signatures and representative code snippets
     for non-obvious logic. Snippets are for illustration — not working implementation code. -->

```typescript
// before
function foo(x: string): void { ... }

// after
function foo(x: string, options?: FooOptions): void { ... }
```

**Rationale:** <!-- Why this approach was chosen -->

**Alternatives considered:**
- Alternative A — rejected because: …

---

### Phase 2: <name>

<!-- Repeat the structure above for each phase. -->

---

## Decision Log

<!-- Append non-obvious decisions made during implementation.
     Format: YYYY-MM-DD — <decision> — <reason>
     Do not populate this section before implementation begins. -->
