# Skill: Debug Session

You are now in a focused debug session. All findings are written to `docs/debug/`.
Do not attempt fixes before the root cause is confirmed.

---

## On Invocation

When this skill fires:

1. **Record the bug slug**: a short kebab-case label for the symptom (e.g. `null-pointer-checkout`)
2. **Create the finding file**: `docs/debug/YYYY-MM-DD-<slug>.md` using `templates/debug/_finding.md`
3. **Fill in Observed Behavior and Expected Behavior** from the user's description before reading any code
4. **State the finding file path** before proceeding

---

## Stage 1 — Reproduce

- Confirm the bug is reproducible from the information given
- If a reproduction step is unclear, ask the human before reading any code
- Copy the exact error message, stack trace, or symptom verbatim into the finding file under **Observed Behavior**
- Do not form a hypothesis yet

---

## Stage 2 — Investigate

Read the relevant files **deeply and in great detail** — do not skim.

- Trace the call chain from the symptom backward toward the source
- List every file read and what was learned from each
- Document the full data flow around the symptom
- Do not hypothesize yet — gather evidence first
- Add any files read to the finding under a **Files Examined** section

---

## Stage 3 — Hypothesize

- State one or more candidate root causes, ranked by likelihood
- Each hypothesis must cite a specific file and line number
- Explain the reasoning chain that leads from symptom to cause
- Do not write any fix yet

---

## Stage 4 — Verify

- Confirm the hypothesis without modifying any code
- Acceptable verification methods: reading more code, tracing data flow, reviewing test output, reading logs
- If the hypothesis is wrong: return to Stage 2 and re-investigate
- Document the confirmed root cause in the finding file under **Root Cause**
- Include: file path, line number, and a clear explanation of why this occurs

---

## Stage 5 — Fix

- Apply the minimal change that addresses the confirmed root cause — no opportunistic cleanup
- Run lint, type check, and the tests most relevant to the affected code after fixing
- If tests fail: revert with `git checkout -- <affected files>` and return to Stage 3
- Do not modify files outside the confirmed scope — surface that to the human instead

---

## Stage 6 — Document

- Complete all sections of the finding file
- Record the fix under **Fix Applied** (or `Fix pending — see plan <NNN>` if it warrants a full feature plan)
- Note anything discovered but not fixed under **Out-of-Scope Observations**
- Set finding status to `RESOLVED` or `DEFERRED`
- Present the completed finding file to the human

---

## Hard Constraints

- Do not write any fix before Stage 4 confirms the root cause
- Do not clean up surrounding code while fixing — one change, one purpose
- If the fix requires a design change rather than a targeted correction, stop and suggest opening a FEATURE session
- Never skip the finding document — it is what makes the debug session resumable and auditable
- If this bug reveals a systemic pattern, note it in **Out-of-Scope Observations** rather than expanding scope
