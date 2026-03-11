# Validation Checklist: workflow skill

Use before distributing or after any structural change. Based on
Anthropic's official skill guide (Reference A: Quick Checklist).

---

## Structure Compliance

- [ ] `SKILL.md` exists at repo root
- [ ] File name is exactly `SKILL.md` (case-sensitive — not skill.md, SKILL.MD, etc.)
- [ ] `ls -1 | grep SKILL` returns exactly `SKILL.md`
- [ ] No `README.md` inside the skill folder — `README.md` is GitHub-facing only
- [ ] `templates/` directory does not exist (replaced by `assets/`)
- [ ] `assets/plans/` contains `_research.md`, `_plan.md`, `_todo.md`
- [ ] `assets/debug/` contains `_finding.md`
- [ ] `references/` contains `overview.md` and `onboarding.md`
- [ ] `evals/` contains `trigger-tests.md`, `functional-tests.md`, `validation-checklist.md`
- [ ] `docs/plans/.gitkeep` exists (runtime output dir preserved)
- [ ] `docs/debug/.gitkeep` exists (runtime output dir preserved)
- [ ] No `scripts/` directory (no executable code needed)

---

## YAML Frontmatter

- [ ] `SKILL.md` opens with `---` on line 1
- [ ] Frontmatter closes with `---`
- [ ] `name` field is present and kebab-case
- [ ] `name` contains no spaces or capitals
- [ ] `name` matches the folder name (`workflow`)
- [ ] `name` does not contain "claude" or "anthropic" (reserved)
- [ ] `description` field is present
- [ ] `description` includes WHAT the skill does
- [ ] `description` includes WHEN trigger phrases (user-facing language)
- [ ] `description` is under 1024 characters: `echo -n "$(description value)" | wc -c`
- [ ] No XML angle brackets (`<` or `>`) in the frontmatter block
- [ ] `license` field is set (MIT)
- [ ] `metadata.author` is set
- [ ] `metadata.version` is set

---

## Cross-Reference Integrity

- [ ] No `templates/` references anywhere: `grep -r "templates/" . --include="*.md"`
- [ ] All `assets/` references in `.claude/commands/` resolve to existing files
- [ ] All `assets/` references in `.claude/profiles/` resolve to existing files
- [ ] `references/onboarding.md` uses `assets/` in all `cp` commands
- [ ] `README.md` documentation links point to `references/` not `docs/`
- [ ] `CHANGELOG.md` path entries use `assets/` not `templates/`

---

## Functional Gates

- [ ] `/workflow` refuses to implement when `plan.md` status is `PENDING REVIEW`
- [ ] `/workflow` creates `research.md` before any code is written
- [ ] `/debug` creates finding file before reading any source files
- [ ] `/refactor` defines scope before reading any code
- [ ] Orchestrator passes only declared-phase files to sub-agents (not full plan)
- [ ] `todo.md` is only written by the orchestrator (not implementers or testers)

---

## Progressive Disclosure

- [ ] `SKILL.md` body does not duplicate content from `.claude/commands/` verbatim
- [ ] `SKILL.md` body links to `references/` for detailed documentation
- [ ] `SKILL.md` body links to `.claude/commands/` for full protocols
- [ ] `SKILL.md` word count is under 5,000 words: `wc -w SKILL.md`

---

## After Any Structural Change

- [ ] Run trigger tests: `evals/trigger-tests.md`
- [ ] Run functional tests for affected skill: `evals/functional-tests.md`
- [ ] Update `CHANGELOG.md` with the version bump and change description
- [ ] Update `metadata.version` in `SKILL.md` frontmatter
