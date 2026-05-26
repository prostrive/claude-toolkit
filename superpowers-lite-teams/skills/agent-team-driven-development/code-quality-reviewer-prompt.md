# Code Quality Reviewer Prompt Template

Dispatch as a **fresh, one-shot subagent** (no `team_name`) using `superpowers-lite-teams:code-reviewer` agent type, for unbiased quality review.

**Purpose:** Verify implementation is well-built — clean, tested, maintainable.

**Only dispatch after spec compliance review passes (`SPEC COMPLIANT`).**

```
Agent tool:
  subagent_type: superpowers-lite-teams:code-reviewer
  description: "Code quality review: [<role>] Task N"
  prompt: |
    Review the implementation of [<role>] Task N: <task name>

    ## What Was Implemented

    <From implementer's completion report — Summary and Journal block.>

    ## Scope / Intent

    <One or two sentences on what this code is supposed to do at a high level. Use this only to understand intent — spec compliance has already been verified by the spec reviewer in the prior stage.>

    ## Commits to Review

    Base SHA: <tip of base branch at task start — parent of first task commit>
    Head SHA: <HEAD after last task commit>

    Use `git diff <base>..<head>` and `git log <base>..<head>` to see
    exactly what changed and in what order.

    Review the diff for:
    - Bugs and logic errors
    - Security vulnerabilities
    - Code quality and maintainability
    - Test coverage and test quality
    - Adherence to project conventions
    - Naming clarity
    - Unnecessary complexity

    ## Focus

    High-confidence issues that truly matter. Don't nitpick style if
    it follows project conventions.

    Test-quality findings (tests that don't assert, missing edge-case coverage) count as high-confidence even when the test suite is green — those are exactly the issues that survive spec review.

    ## Report Format

    Follow the `requesting-code-review` methodology for the review itself. For the report format, use exactly the structure below (it takes precedence over any conflicting format guidance from that skill).
    Use file:line references throughout so the lead can forward your
    report verbatim to the implementer.

    Exactly one of these outcomes at the end:
    - `APPROVED` — implementation is well-built
    - `ISSUES FOUND` — list each issue with severity (Critical / Important / Minor),
      file:line reference, and suggested fix

    **Outcome rule:** Use `APPROVED` only when there are zero Critical or Important findings. Minor-only findings still go under `ISSUES FOUND` so the lead can decide whether to forward them.
```
