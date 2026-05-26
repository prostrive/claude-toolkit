# Journal Snapshot Template

Format for the end-of-session journal file written at Phase 4 before `TeamDelete`.

## File Location

`docs/superpowers/plans/<plan-slug>.journal.md`

Where `<plan-slug>` is the slug derived from the plan filename (e.g., `2026-05-02-agent-teams-skills`). If `TeamCreate` required a session-date suffix to avoid slug collision, the journal filename still uses the original plan slug — not the collision-mangled team slug — so plan and journal filenames match.

## How to Generate

Walk completed tasks in plan order. For each one, call `TaskGet` on the task to retrieve its `description` field. The description contains the original task text followed by the appended Completion Report block (see SKILL.md Phase 3 — the lead concatenates the teammate's full report onto the description at completion time, delimited by a `---` / `## Completion Report` marker). Extract the Status, Summary, Journal, Tests, and Commits from the Completion Report block. Write into the snapshot file in the format below.

**Placeholder conventions:** Angle-bracket tokens like `<plan-slug>` or `<N>` are values to fill in. Square-bracket ellipses like `[... same structure ...]` are instructions to repeat the surrounding pattern. Angle-bracket sentences (like `<If any tasks... Otherwise:>`) are instructions to the lead, not placeholders.

## Format

```markdown
# <Plan Title> — Journal Snapshot

**Generated:** <ISO 8601 timestamp, e.g., `2026-05-02T14:30:00Z`>
**Plan:** `docs/superpowers/plans/<plan-slug>.md`
**Team:** `<plan-slug>`
**Final status:** All <N> tasks completed and reviewed (including any reopened-and-resolved tasks — see Notable Escalations below).

---

## Task 1: <task name>

**Specialist:** <role>
**Status:** DONE | DONE_WITH_CONCERNS (resolved) | DONE_WITH_CONCERNS (noted)
**Summary:** <one-sentence>

**Journal:**
- Files created: <paths>
- Files modified: <paths>
- Exports added: <names>
- Schema/migrations: <if any>
- Patterns established: <notes>
- Notes: <free text>

**Tests:** <N passing>
**Commits:** <SHAs>

---

## Task 2: <task name>

[... same structure ...]

---

[... all tasks in plan order ...]

---

## Notable Escalations

<If any tasks required replacement specialists, reopened-after-final-review,
 or other recovery actions, document them here. Otherwise:>
None.

## Final Cross-Cutting Review

**Reviewer:** superpowers-lite-teams:code-reviewer (final)
**Outcome:** APPROVED | ISSUES ADDRESSED (with details)
**Notes:** <reviewer's summary — paste the final reviewer's findings directly, as there is no separate report file>
```

## Notes

- The snapshot is human-readable reference, not machine-parseable state. Tasks themselves remain the source of truth until `TeamDelete` clears them.
- Include any `DONE_WITH_CONCERNS` observations that were merely noted (not addressed) so future work has the context.
- If a task was reopened after the final reviewer caught an issue, note that explicitly under "Notable Escalations" so the pattern is visible to you and future sessions.
- `DONE_WITH_CONCERNS (noted)` means the teammate flagged observations that were recorded but not addressed (e.g., "this file is getting large"). Those observations appear in the task's Journal → Notes field.
