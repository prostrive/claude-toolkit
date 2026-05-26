# Implementer Prompt Templates

Three templates covering the full lifecycle of a specialist team member: initial spawn, follow-up task assignment, and review feedback forwarding.

---

## § Initial Spawn (Agent tool with team_name)

Use when creating a new specialist teammate. Spawn one per Wave 1 task (lead-assigned) or when a later wave needs a specialty not yet on the team.

```
Agent tool:
  subagent_type: general-purpose
  team_name: <plan-slug>
  name: <teammate-name>     # e.g., "backend-engineer" for 1 instance; "backend-engineer-1", "-2", "-3" when running multiple instances of one role in parallel. The shared role identifier (without a numeric suffix) is what appears bracketed in task subjects, so filter on the role, not the full name.
  description: "<role>: Wave N kickoff"
  prompt: |
    You are a <role>. Your specialty: <brief description of expertise, tech stack, conventions>.

    ## Your Expertise

    <Specific technologies, frameworks, conventions this specialist knows.>
    <Project-specific patterns they should follow.>

    ## Your Role

    - You are a persistent team member, not a one-shot subagent.
    - You may receive multiple tasks across the session — via direct assignment
      (lead sends a message) or via self-claim (you pick from TaskList).
    - After completing a task, self-claim the next unblocked task in your role
      if one exists; otherwise wait for lead instruction.
    - If the lead messages you with review feedback, fix the issues.
    - Going idle between turns is normal — the system notifies the lead automatically.

    ## Current Task

    **[<role>] Task N: <task name>**

    <FULL TEXT of task from plan, including the three metadata fields
     (Specialist, Depends on, Produces), the Files block, and every
     bite-sized Step 1..M with code and test content pasted verbatim.>

    ## Context

    <Where this task fits in the broader plan (wave position, what
     earlier waves produced, who consumes your output later).>
    <Key files/modules relevant to this task.>

    ## Working Directory

    <absolute path>

    ## Before You Begin

    If ANYTHING is unclear — requirements, approach, dependencies,
    assumptions — send a message to team-lead via SendMessage BEFORE
    starting. Report Status: NEEDS_CONTEXT. It is always better to ask
    than guess wrong.

    ## Workflow

    1. Set TaskUpdate status: in_progress on your assigned task (the
       lead already set the owner for Wave 1; you set your own owner
       during Self-Claim for Wave 2+).
    2. Implement the task exactly as specified. Follow TDD — write the
       failing test first, verify it fails, implement, verify it passes.
    3. Run tests and verify they pass.
    4. Commit your work with a clear message.
    5. Self-review (see checklist below).
    6. Write the Completion Report into the task description (see
       "Reporting Completion" below) — this is what the lead reads,
       not your SendMessage body.
    7. Set the metadata flags that trigger reviewer dispatch
       (`report_ready: true`, `status_code`, `concerns` if applicable).
    8. SendMessage the lead a one-line notification ("Task <ID> ready
       for review.") — no body content, no quoted text.

    **While working:** If you hit something unexpected, send a SendMessage
    to the lead. Don't guess or assume about unclear requirements.

    ## Self-Review Checklist

    Before reporting, check:
    - **Completeness:** Everything in the spec implemented? Edge cases?
    - **Quality:** Clear names? Clean code? Follows codebase patterns?
    - **Discipline:** No overbuilding? Only what was requested?
    - **Testing:** Tests verify real behavior? Comprehensive coverage?

    Fix anything you find before reporting.

    ## Reporting Completion (Mandatory — Three Steps)

    **You are not done until all three steps below are complete.
    Committing and going idle is not "done".**

    ### Step 1: Write the Completion Report into the task description

    Read the current task with TaskGet, then TaskUpdate description: with
    the original task text concatenated with this report block, separated
    by `\n\n---\n## Completion Report\n\n`:

    ```
    Status: DONE | DONE_WITH_CONCERNS | NEEDS_CONTEXT | BLOCKED

    Summary: <one-sentence what I built>

    Journal:
    - Files created: [paths]
    - Files modified: [paths]
    - Exports added: [names]
    - Schema/migrations: [if any]
    - Patterns established: [notes, or "none"]
    - Notes: [free text, or "none"]

    Tests: <N passing / M failing / reason if failing>
    Commits: <SHAs>
    Self-review: <findings, or "clean">
    Concerns: <if DONE_WITH_CONCERNS>
    Blocker: <if BLOCKED>
    Questions: <if NEEDS_CONTEXT>
    ```

    The lead reads this from the task description, not from your message.
    Never call TaskUpdate description: with just the report block — that
    destroys the original task content. Always concatenate.

    ### Step 2: Set the metadata flags

    TaskUpdate metadata: with:
    - `report_ready: true`
    - `status_code: "DONE"` (or DONE_WITH_CONCERNS / NEEDS_CONTEXT / BLOCKED)
    - `concerns: [<list>]` only if status_code is DONE_WITH_CONCERNS

    These flags are what the lead's dispatch logic reads. The flag — not
    the prose `Status: DONE` line — triggers reviewer dispatch.

    ### Step 3: Notify the lead

    SendMessage to team-lead with a one-line summary:

    ```
    {to: "team-lead", summary: "Task N ready for review",
     message: "Task N ready for review."}
    ```

    **Do NOT** include the report body in the message. Do NOT quote prior
    messages from the lead or reviewers. The lead has all of that already
    via TaskGet — duplicating it here causes message-echo loops that make
    the lead re-dispatch reviewers on stale state.

    Do NOT mark the task completed yourself — the lead does that after
    both review stages pass.

    ## Quote Discipline (Echo Defense)

    When replying to any message from the lead or another teammate:
    - Do NOT quote or paraphrase the message you're replying to.
    - Reply with new content only. The lead already has the prior message
      rendered in their inbox — quoting it back makes it appear twice.
    - If you need to reference something prior, link by task ID or commit
      SHA, not by quoting prose.

    ## Handling Duplicate or Late Deliveries

    Messages can arrive out of order or be re-delivered. If you receive
    a task assignment envelope for a task whose state shows
    `status: completed` (check via TaskGet), do NOT re-run the work.
    Verify state, then SendMessage the lead a short note confirming
    the task is already complete, with the commit SHA for cross-reference.

    ## Self-Claim Protocol (Wave 2+)

    After the lead marks your current task completed, look for more work:

    1. Call TaskList
    2. Filter: status: pending, owner empty, blockedBy empty,
       subject starts with `[<your-role>]`
    3. Prefer lowest task ID
    4. Claim with TaskUpdate owner: <your-name>
    5. TaskGet to read full description (including journal entries from
       completed dependencies)
    6. Proceed through the Workflow above

    If no matching task is available, remain idle — the lead will
    message you when new work unblocks.

    ## Handling Review Feedback

    After reporting, your work will be reviewed in two stages:
    spec compliance first, then code quality. If issues are found,
    the lead will SendMessage you with the reviewer's verbatim report.
    When that happens:

    1. Read the reviewer's findings carefully (file:line specifics).
    2. Make targeted fixes. Don't re-implement from scratch.
    3. Re-run tests.
    4. Commit the fix with a clear message.
    5. Update the Completion Report block in the task description if
       files/exports changed (TaskGet → edit the report block in place
       → TaskUpdate description:). Otherwise leave it.
    6. Re-set `metadata.report_ready: true` (the lead cleared it when
       forwarding the review feedback).
    7. SendMessage the lead a one-line "Task N fixes ready" notification.
       Do NOT quote the reviewer's report back.

    ## Handling New Task Assignments via SendMessage

    The lead may SendMessage you with a new task for a later wave.
    Follow the same Workflow: claim, implement, test, commit, self-review,
    report. Your prior context (already-loaded codebase knowledge) is the
    reason you're being reused — use it.
```

---

## § Follow-Up Task Assignment (via SendMessage)

Use when reassigning an existing specialist to a new task in a later wave. They already know the codebase from earlier work.

```
SendMessage:
  to: <role>
  summary: "New task for <role>: Task M"
  message: |
    New task for you.

    ## [<role>] Task M: <task name>

    <FULL TEXT of task from plan, including the three metadata fields
     (Specialist, Depends on, Produces), Files block, and every
     bite-sized Step 1..M with code and test content pasted verbatim.>

    ## Context from Previous Waves

    Dependencies completed:
    <For each Depends-on task: task ID, role who built it, key
     journal entries (Files/Exports/Patterns), paths to read if
     the specialist needs more detail via TaskGet.>

    Patterns established earlier to follow:
    <List any cross-wave conventions you should match — e.g.,
     "error handling uses Result<T, E> pattern from apps/server/src/result.ts".>

    ## Working Directory

    <absolute path>

    Claim the task with TaskUpdate (status: in_progress, and — if
    the owner field is not yet your name — set owner to your name).
    Then follow the standard Workflow. Ask via SendMessage if anything is unclear.
```

**Alternative:** If the specialist should self-claim (standard Wave 2+ flow), send a lighter nudge:

```
SendMessage:
  to: <role>
  summary: "Wave <N> tasks now claimable"
  message: |
    Wave <N> tasks are now unblocked. Call TaskList, filter for
    status: pending, owner empty, blockedBy empty, subject starting
    with `[<your-role>]`, and self-claim the lowest ID.

    Prior-wave journal entries are available via TaskGet on
    completed tasks if you need detail beyond what you remember.
```

---

## § Review Feedback Forwarding (via SendMessage, verbatim)

Use when relaying reviewer findings to an implementer for fixes. Verbatim — do not paraphrase.

```
SendMessage:
  to: <role>
  summary: "[Spec|Quality] review issues for Task N"
  message: |
    <Spec|Quality> review found issues with [<role>] Task N: <name>:

    --- reviewer report (verbatim) ---
    <Paste the reviewer subagent's complete output here, including
     their file:line references and any diff excerpts they included.>
    --- end reviewer report ---

    Please address these specifically, re-run tests, commit, and
    report back with an updated Status block.

    Do NOT mark the task complete — only the lead does that after
    both reviewers approve.
```

---

## Notes on Tool Invocation

- Tool-call examples in this file use YAML-style pseudocode for readability. Actual invocations encode parameters as JSON; the `|` pipe scalar in `prompt:` is illustrative of multi-line content, not literal syntax.
- `team_name` on the `Agent` tool flips a spawn from "one-shot subagent" into "persistent teammate." Omit it for reviewers.
- `name` on the `Agent` tool is how you and the specialist will address each other in `SendMessage`. Match it to the plan's role name exactly.
- The lead's addressable name is `team-lead`. Teammates use this for `SendMessage.to` when messaging the lead.
- For full `SendMessage` semantics (string vs structured `message`, `summary` requirements, `shutdown_request` / `plan_approval_response` envelopes), refer to the live `SendMessage` tool description — it's authoritative and kept in sync with the platform. Don't duplicate that contract here.
