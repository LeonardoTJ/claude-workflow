# Trigger Tests: workflow

Use these test cases to verify the skill loads at the right times based on
the YAML `description` field. Target: triggers on 90%+ of "Should trigger"
cases without manual invocation.

---

## Should Trigger

Session type declarations:
- `"Session type: FEATURE"`
- `"Session type: REFACTOR"`
- `"Session type: DEBUG"`
- `"Session type: ORCHESTRATE"`

Task-start phrases:
- `"I need to start working on a new feature"`
- `"Let's plan this change before implementing"`
- `"Help me research the checkout module"`
- `"I need a plan for adding authentication"`
- `"New session — building the payment integration"`
- `"Resume session docs/plans/003-auth-refactor/"`

Explicit invocations:
- `"/workflow"`
- `"/debug"`
- `"/refactor"`

Problem-framing phrases:
- `"Investigate this bug in the cart controller"`
- `"I want to refactor the user service"`
- `"Start a structured workflow for this task"`

---

## Should NOT Trigger

- `"What does this function do?"` — quick lookup, no session needed
- `"Fix this typo in line 12"` — trivial, no planning needed
- `"What's the difference between async and await?"` — general knowledge
- `"Write a Python function to sort a list"` — generic coding task
- `"Summarize this file"` — read-only, no workflow needed
- `"What is the capital of France?"` — unrelated

---

## How to Test

**Manual (Claude Code):**
1. Open Claude Code with the skill installed
2. Type each "Should trigger" phrase in a new conversation
3. Observe whether the skill loads (visible in the skill indicator) before Claude responds
4. Repeat for "Should NOT trigger" phrases — skill must not load

**Debugging undertriggering:**
Ask Claude: *"When would you use the workflow skill?"* — Claude will quote
the description back. Adjust trigger phrases in the YAML `description` if
the response doesn't cover your expected use cases.

**Debugging overtriggering:**
Add negative qualifiers to the `description` field:
```yaml
description: "... Do NOT use for quick lookups, one-line fixes, or general coding questions."
```
