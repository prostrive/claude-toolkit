---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code. Branches on the fast/middle/full lane from using-superpowers.
---

# Writing Plans

## Lane Branch (superpowers-lite)

**Full lane:** the full plan-doc workflow below — committed plan in `docs/superpowers/plans/`, complete code in every step, self-review pass, two execution-path options.

**Middle lane:** the **Middle-Lane Short Plan** section at the bottom of this skill — one-screen committed plan doc, no per-step pre-written code, no self-review loop, no execution-path question. Single reviewer pass runs at the end of the whole change, not per-task.

**Fast lane:** skip the committed plan doc entirely. Use the **Fast-Lane TodoWrite Plan** section at the bottom of this skill — an in-conversation TodoWrite list, no markdown file, no self-review loop. Implementation begins immediately after the TodoWrite is created.

If `using-superpowers` did not set a lane, return to `using-superpowers` and have the user pick. Do not pick yourself.

## Overview

Write comprehensive implementation plans assuming the engineer has zero context for our codebase and questionable taste. Document everything they need to know: which files to touch for each task, code, testing, docs they might need to check, how to test it. Give them the whole plan as bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

Assume they are a skilled developer, but know almost nothing about our toolset or problem domain. Assume they don't know good test design very well.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Context:** If working in an isolated worktree, it should have been created via the `superpowers:using-git-worktrees` skill at execution time.

**Save plans to:** `docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md`
- (User preferences for plan location override this default)
- (Middle lane: still save here, but use the short-plan structure — see bottom of skill)
- (Fast lane: skip this — use a TodoWrite list instead, see bottom of skill)

## Scope Check

If the spec covers multiple independent subsystems, it should have been broken into sub-project specs during brainstorming. If it wasn't, suggest breaking this into separate plans — one per subsystem. Each plan should produce working, testable software on its own.

## File Structure

Before defining tasks, map out which files will be created or modified and what each one is responsible for. This is where decomposition decisions get locked in.

- Design units with clear boundaries and well-defined interfaces. Each file should have one clear responsibility.
- You reason best about code you can hold in context at once, and your edits are more reliable when files are focused. Prefer smaller, focused files over large ones that do too much.
- Files that change together should live together. Split by responsibility, not by technical layer.
- In existing codebases, follow established patterns. If the codebase uses large files, don't unilaterally restructure - but if a file you're modifying has grown unwieldy, including a split in the plan is reasonable.

This structure informs the task decomposition. Each task should produce self-contained changes that make sense independently.

## Bite-Sized Task Granularity

**Each step is one action (2-5 minutes):**
- "Write the failing test" - step
- "Run it to make sure it fails" - step
- "Implement the minimal code to make the test pass" - step
- "Run the tests and make sure they pass" - step
- "Commit" - step

## Plan Document Header

**Every plan MUST start with this header:**

```markdown
# [Feature Name] Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

---
```

## Task Structure

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

- [ ] **Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

- [ ] **Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

- [ ] **Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

- [ ] **Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

- [ ] **Step 5: Commit**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
````

## No Placeholders

Every step must contain the actual content an engineer needs. These are **plan failures** — never write them:
- "TBD", "TODO", "implement later", "fill in details"
- "Add appropriate error handling" / "add validation" / "handle edge cases"
- "Write tests for the above" (without actual test code)
- "Similar to Task N" (repeat the code — the engineer may be reading tasks out of order)
- Steps that describe what to do without showing how (code blocks required for code steps)
- References to types, functions, or methods not defined in any task

## Remember
- Exact file paths always
- Complete code in every step — if a step changes code, show the code
- Exact commands with expected output
- DRY, YAGNI, TDD, frequent commits

## Self-Review

After writing the complete plan, look at the spec with fresh eyes and check the plan against it. This is a checklist you run yourself — not a subagent dispatch.

**1. Spec coverage:** Skim each section/requirement in the spec. Can you point to a task that implements it? List any gaps.

**2. Placeholder scan:** Search your plan for red flags — any of the patterns from the "No Placeholders" section above. Fix them.

**3. Type consistency:** Do the types, method signatures, and property names you used in later tasks match what you defined in earlier tasks? A function called `clearLayers()` in Task 3 but `clearFullLayers()` in Task 7 is a bug.

If you find issues, fix them inline. No need to re-review — just fix and move on. If you find a spec requirement with no task, add the task.

## Execution Handoff

After saving the plan, offer execution choice:

**"Plan complete and saved to `docs/superpowers/plans/<filename>.md`. Two execution options:**

**1. Subagent-Driven (recommended)** - I dispatch a fresh subagent per task, review between tasks, fast iteration

**2. Inline Execution** - Execute tasks in this session using executing-plans, batch execution with checkpoints

**Which approach?"**

**If Subagent-Driven chosen:**
- **REQUIRED SUB-SKILL:** Use superpowers:subagent-driven-development
- Fresh subagent per task + two-stage review

**If Inline Execution chosen:**
- **REQUIRED SUB-SKILL:** Use superpowers:executing-plans
- Batch execution with checkpoints for review

---

## Middle-Lane Short Plan (superpowers-lite)

Use this when `using-superpowers` selected the middle lane. The change is multi-step and worth writing down, but the design is settled and full plan-doc ceremony costs more than it returns.

### Process

1. **Quick context scan** — read the file(s) the task touches and any callers that affect the change. Skip broad project-wide exploration unless the call graph requires it.

2. **Ask any genuinely needed clarifying questions** — middle lane skipped brainstorming, so this is your one chance. Batch them in one consolidated message via AskUserQuestion. Skip entirely if you have no real uncertainty.

3. **Write a one-screen plan doc** to `docs/superpowers/plans/YYYY-MM-DD-<topic>-plan.md`. Commit it. Structure:

```markdown
# [Feature/Change Name] Short Plan (middle lane)

**Goal:** [one sentence]

**Files to touch:**
- `path/to/file-a.ts` — [1-line responsibility]
- `path/to/file-b.ts` — [1-line responsibility]
- `tests/path/file-a.test.ts` — [what it tests]

**Steps:**
1. Read `file-a.ts` and its callers
2. Write failing test for [behavior] in `tests/path/file-a.test.ts`
3. Implement change in `file-a.ts`
4. Run tests, fix until green
5. Apply [follow-on] in `file-b.ts`
6. Run full test suite + lint/typecheck
7. Commit
8. Dispatch end-of-change reviewer subagent (see subagent-driven-development middle-lane mode)

**Test:** [the test or tests that cover this change — concrete enough that a reviewer can find them]
```

Typical middle-lane plans are 5–15 steps.

4. **No pre-written code in the plan.** You don't need to ship the code in the plan because you're about to write it for real. Keep the plan to file paths, step descriptions, and the testing story.

5. **No self-review pass. No execution-path choice presented to the user.** The user already picked middle lane; the plan is the plan.

6. **Execute the plan.** Two options at your discretion (no need to ask the user):
   - **Inline in the main session** — work through the steps, commit after passing tests.
   - **Subagent implementer** — dispatch a single fresh implementer subagent with the plan and the task. Pick this when the plan has 8+ mechanical steps and dispatching saves your context window. See `subagent-driven-development` middle-lane mode.

7. **Single combined reviewer pass at the end.** After all steps complete and tests pass, dispatch one reviewer subagent that checks both spec compliance and code quality against the whole change. Fix anything it flags. No per-task two-stage review. This single pass is the only review checkpoint — skipping it means no review.

### When to bail out of the middle lane

If during plan writing or execution you find any of these, STOP and ask the user (via AskUserQuestion) whether to reclassify to full lane:

- A genuine design fork emerges (two viable architectures with different long-term implications)
- A new dependency or new piece of infrastructure is required
- The change crosses a service or team boundary
- The change touches a public API or schema **and the project is post-launch** (pre-launch stays middle)
- The plan grows past ~15 steps or starts touching files you didn't expect

Don't silently escalate ceremony, and don't silently push through with under-ceremony.

### What stays mandatory

- **TDD** (with the same exceptions as fast lane — see `test-driven-development`).
- **Verification before completion.** Run the tests, run lint/typecheck, confirm the change works.
- **Commits.** Frequent commits, not one big commit at the end.
- **The end-of-change reviewer pass.** It's the only review you get.

---

## Fast-Lane TodoWrite Plan (superpowers-lite)

Use this when `using-superpowers` selected the fast lane. The committed plan doc is overkill when the entire change fits on a single screen.

### Process

1. **Create a TodoWrite list** with the actual steps. Each todo is a real action, not a phase. Typical fast-lane shape (3–6 items):
   - Read [the file(s) being changed]
   - Write the failing test for [behavior]
   - Implement the change in [file]
   - Verify tests pass and lint is clean
   - Commit

2. **No markdown plan file. No execution-path question.** The fast lane always executes inline in the current session. There is no subagent-driven mode for fast-lane work — the overhead of dispatching subagents outweighs the benefit on a 5-minute change.

3. **No "no placeholders" enforcement.** The plan is in your head and in TodoWrite; you don't need to pre-write every line of code in the plan because you're about to write it for real in the next step.

4. **Self-review collapses to a single sanity check.** Before starting implementation, glance at the TodoWrite and ask: "did I forget a test? does this actually cover what the user asked for?" If yes, fix the TodoWrite. If no, start.

### When to bail out of the fast lane

If while writing the TodoWrite you find yourself with more than ~8 todos, or you notice the change is touching files you didn't expect, STOP. The task is bigger than fast-lane work. Ask the user via AskUserQuestion whether to reclassify to middle or full lane — do not pick yourself.

### What stays mandatory

- **A test for the change.** Fast lane is not "skip tests" — see `test-driven-development` fast-lane behavior.
- **Verification before completion.** Run the test. Run the lint/typecheck. Confirm the thing works.
- **A commit.** Fast-lane work still commits.
