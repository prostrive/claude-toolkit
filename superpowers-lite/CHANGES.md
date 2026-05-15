# superpowers-lite — divergence from upstream

Forked from [obra/superpowers](https://github.com/obra/superpowers) v5.1.0.

The methodology is the same. The only changes are five tweaks that introduce a **fast lane / full lane** decision so small tasks can skip the heavyweight design-doc + plan-doc + multi-subagent ceremony that upstream applies uniformly.

Full lane is bit-for-bit upstream behavior. Fast lane is the new mode.

## Lane decision criteria

Decided in `skills/using-superpowers/SKILL.md` before any other skill is invoked.

**Fast lane** — ALL must be true:
- Touches ≤2 files (or one self-contained component)
- No public API change, no schema / migration change
- No new infrastructure, no new dependency
- No cross-service coordination

**Full lane** — any of: multiple files, API/schema changes, new dependency, cross-service coordination, or user explicitly asks for a design doc.

If unsure → default to full lane.

## The 5 tweaks

### 1. `using-superpowers/SKILL.md` — adds the lane decision

New "Lane Decision (superpowers-lite)" section. Claude must announce `Lane: fast` or `Lane: full` before invoking any downstream skill. User can override with `"just do it"` or `"let's plan this properly"`.

### 2. `brainstorming/SKILL.md` — fast-lane mini-brainstorm

- HARD-GATE is split: full lane keeps it as-is; fast lane runs a single consolidated message with batched questions + proposed approach + inline approval, then implementation begins.
- New section at the bottom: **Fast-Lane Mini-Brainstorm**. No spec doc. No self-review. No separate user-review gate.
- Bail-out clause: if hidden complexity emerges (new dependency, schema change, multi-file scope), STOP and switch to full lane.

### 3. `writing-plans/SKILL.md` — TodoWrite plan for fast lane

- Lane branch at the top of the skill.
- New section: **Fast-Lane TodoWrite Plan**. No committed `docs/superpowers/plans/*.md`. No execution-path choice (fast lane always executes inline). No "no placeholders" enforcement on the in-memory plan.
- Bail-out at ~8 todos: reclassify as full lane.

### 4. `subagent-driven-development/SKILL.md` — fast lane skips this skill entirely

- Fast lane does **not** dispatch implementer / spec-reviewer / code-quality-reviewer subagents. Inline implementation by the main session is the path. `verification-before-completion` at the end is the review.
- New **Reduced-Ceremony Mode** for medium-sized full-lane work: per-task, you can collapse the two-stage review to a single spec-compliance pass if the task touches ≤2 files, has a concrete test in the plan, and is mechanical. Final whole-implementation reviewer still runs.

### 5. `test-driven-development/SKILL.md` — context-aware fast-lane exceptions

TDD remains mandatory in both lanes for anything with behavior. New **Fast-Lane Exceptions** section allows skipping a test-first pass — without asking the human partner — for:

- Pure configuration changes
- Copy / string changes
- Style-only (CSS / Tailwind) changes with no behavior
- Trivial renames inside one module
- Purely declarative DB migrations (column with default, index)

Anything with conditional logic, data transformation, input validation, or user-visible computation result — TDD mandatory regardless of lane.

## What is NOT changed

- The 1%-rule from `using-superpowers` (must invoke any potentially relevant skill).
- `verification-before-completion` — runs in both lanes.
- `systematic-debugging` — debugging discipline is the same regardless of lane.
- `using-git-worktrees`, `finishing-a-development-branch` — same.
- All the other 9 skills are untouched.
- The SessionStart hook mechanism (only the announcement string is updated to "superpowers (lite)").

## How to update from upstream

When upstream releases a new version, the safe merge strategy is:

1. `git diff` upstream's old version vs. new version
2. Apply the diff to `superpowers-lite/` skill-by-skill
3. Re-apply the 5 tweaks above to any skill that upstream touched

Most upstream changes will land cleanly because the tweaks are additive (new sections at the top or bottom of each file, or a `## Lane Branch` near the top, not edits scattered through the body).

## Expected impact

For tasks like our recent tenant ticket and EB-95 (medium-sized, mostly mechanical, single-area-of-codebase): **30–50% wall-clock reduction** on the planning portion of the cycle, with no loss of test coverage or final review quality. The full lane is unchanged for genuinely large user stories where Claude works autonomously for hours.
