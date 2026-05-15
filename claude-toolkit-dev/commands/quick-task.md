---
description: Lightweight feature/change workflow for small, contained work — single command, no subagent fleet, no architecture phase. Use for new endpoints, simple CRUD, contained UI changes, small refactors.
---

# Quick Task

You are a senior developer executing a small, contained change. This command is the **explicit fast lane** of the toolkit — no `code-explorer` / `code-architect` / `code-reviewer` subagent dispatches, no separate architecture phase, no multi-stage review. The discipline (read first, test, verify, commit) stays. The ceremony goes.

## When to use this command (vs. `/build-feature`)

Pick `/quick-task` when ALL of the following are true:

- Touches ≤2 files (or a single self-contained component / endpoint / page)
- No public API contract change, no data-model / schema / migration change
- No new infrastructure, no new dependency
- No cross-service coordination

Typical fits: a new CRUD endpoint behind an existing pattern, a forgot-password page, fixing a styling bug, adding a missing log line, renaming inside one module, a config tweak.

If ANY of the above is false → use `/build-feature` instead. If you start `/quick-task` and discover hidden complexity (more files than expected, schema change, new dependency), STOP and tell the user to re-invoke with `/build-feature`.

## The Workflow

### Step 1: Read the code

Read the 1-2 files the change is about. Don't dispatch explorer subagents — the change is local, the relevant context is local. If you need to find the right file first, use a single `Grep` or `Glob`, not an agent.

If after reading you realize the scope is bigger than `/quick-task` should handle, stop and recommend `/build-feature`.

### Step 2: One consolidated message to the user

Produce **one message** with up to three parts:

- **(a) Clarifying questions** — only if you genuinely have meaningful uncertainty. Batch them together, prefer multiple-choice. Skip this part entirely if you have no real questions.
- **(b) Proposed approach** — 2-6 sentences. Files you'll touch, what changes, what the test will assert. Skip alternative-approach tradeoffs unless there's a real fork.
- **(c) Approval ask** — "Sound good? I'll proceed." or equivalent.

No design doc. No 2-3 architecture variants. No numbered question list spread across multiple messages.

### Step 3: Implement

On approval, implement. Match existing codebase conventions exactly — naming, patterns, error handling, file structure. Don't add abstractions the task doesn't require.

### Step 4: Test

Write a test for the change. Run it. Confirm it passes.

**Test-first is required for anything with behavior** — conditional logic, data transforms, input validation, computation that produces user-visible output. Write the failing test, watch it fail, then implement.

**Test-first may be skipped, without asking, for these categories only** (the only fast-lane exceptions):
- Pure configuration changes (flipping a flag, changing a value in YAML/JSON)
- Copy / string changes (user-facing text, error messages, labels)
- Style-only changes (CSS / Tailwind / layout with no behavior)
- Trivial local renames where the type-checker is the proof
- Purely declarative migrations (adding a column with a default, adding an index)

"It's a one-liner" is not on the exception list. "I'll add the test after" is not on the list.

### Step 5: Verify

- Run the test you wrote
- Run the project's lint / typecheck if there is one
- For UI changes: spot-check in a browser (start the dev server if needed)
- Confirm the original ask is actually satisfied

If verification fails, fix it. Do not declare done until verification passes.

### Step 6: Brief summary

Two or three lines, max. What changed, where, what's next (if anything). No ceremonial summary template.

## Rules

- **No subagents.** This command's whole point is to avoid the subagent-fleet overhead. If you find yourself wanting to dispatch one, you're probably in `/build-feature` territory — stop and recommend it.
- **No design doc, no architecture phase.** The change is too small to be worth the ceremony.
- **Read before you change.** Fast lane never means "guess at the codebase."
- **Test before you commit.** Fast lane never means "skip tests."
- **One message to the user before implementation.** Not a numbered Q&A across multiple turns.
- **Bail out cleanly.** If the task turns out to be bigger than this command handles, stop and recommend `/build-feature`. Don't try to scale this command up.
