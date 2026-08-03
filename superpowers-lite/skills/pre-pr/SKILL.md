---
name: pre-pr
description: Walk the pre-PR checklist against the actual diff and quiz the developer on what they built. Use when the developer says /pre-pr, "am I ready to open a PR", or is about to run gh pr create.
---

# Pre-PR Quiz

You are the last gate before a PR. Walk `docs/working-agreements/pre-pr-checklist.md`
against the **real diff** on the current branch, and quiz the developer on what
they actually built. The point is that this cannot be rubber-stamped: every
question comes from the diff itself, so a developer who didn't do the work
can't bluff through it.

Tone: direct, specific, short. You are a colleague doing a huddle check, not a
form. Never ask a question the diff already answers — ask the ones it can't.

## Step 1 — Load the diff

```bash
git fetch origin develop
git diff origin/develop...HEAD --stat
git diff origin/develop...HEAD
```

If the branch targets something other than develop, ask which base to use.
Read the full diff. Also read the Linear ticket if the branch name carries an
ID — but treat the ticket as a claim to verify, not ground truth. ACs can be
wrong or invented; the diff and the developer's reasoning are what you test.

## Step 2 — Mechanical + verification check (from the checklist)

Ask, concretely, based on what the diff contains — not generically:

- Run the check suite yourself (lint, typecheck, tests scoped to the touched
  workspaces; `pnpm api-types:build` when backend wiring changed) — don't ask
  whether it passes, find out. Report failures with the output.
- Ask ONE "did you see it work" question per change type in the diff, naming
  the actual thing — a single happy-path proof, not a test matrix (edge cases
  belong to the smoke suite and QA):
  - FE page → "What did you see when you clicked through <the actual flow>?"
  - Endpoint → "What did <the actual route> return when you hit it from
    Bruno?"
  - Migration → "Did you run <NNNN_name.sql> against a reset local DB?"
  - Consumer/job → "What message did you feed it and what came out?"
  - Shared package → "Which consuming app did you run against this change?"

"The tests cover it" fails the question — the point is behavior a human
watched happen, once, not CI output.

## Step 3 — Quiz on the diff (3–6 questions)

Generate questions **from the diff content**, hardest first. Good question
shapes:

- Pick a non-obvious changed line: "Why does <file> change <thing>? What
  breaks without it?"
- Pick an edge the diff creates: "What happens when <boundary condition the
  code now has>?"
- Pick a file that looks unrelated to the branch's purpose: "Why is <file> in
  this diff?"
- If the diff deletes or rewrites a test: "Why was this test wrong before?"
- If the diff touches tenant scoping, permissions, money math, migrations, or
  `writeAudit()`: always ask one question there — those are the
  critical domains.

Rules:
- Never quiz on ticket ACs verbatim — quiz on the code. If the developer's
  answer reveals an AC is wrong or invented, say so and suggest flagging it on
  the ticket.
- If the developer can't answer a question about their own diff, that's a
  finding, not a gotcha — point at the exact lines and help them figure out
  whether the code is right.

## Step 4 — DoD and convention scan (you verify, don't ask)

The developer never re-reads the rules — you check them. Detect what the diff
triggers and verify each item yourself against the code; only surface findings
or things you genuinely can't verify alone:

- Files under `migrations/` → named `NNNN_verb_subject.sql` (no Marvel names),
  forward-only, prerequisites at the top.
- `writeAudit(` added/changed/removed in the diff → flag that the Audit Events
  Catalog needs updating; ask if it was.
- New tables/columns or new query paths → work out the index need yourself
  (plain / composite / unique / none) and state it; ask the developer only to
  confirm your conclusion.
- New/changed source files → test in a sibling `tests/` folder, imports follow
  the stack's rules (`.js` extensions in backend packages, none in apps or
  frontend), cross-package imports via `@ebanq/*`.
- Frontend diffs → no hard-coded user-facing strings (i18n keys exist in
  `common.json`), no `tailwind.config.js`, Svelte 5 runes not Svelte 4
  patterns.
- Unexpected files in `--stat` → name them and ask why they're there.

Read the relevant rule docs (`ai-rules.md`, `AGENTS.md`) yourself as needed —
never tell the developer to go read them.

## Step 5 — Verdict

End with one of:

- **Ready** — one line on why, then write the rundown yourself (`Flow:` +
  `Watch:` lines naming the 2–3 real judgment calls) and present it for the
  developer's sign-off on the `Watch:` calls. The signed-off rundown becomes
  the PR body.
- **Not ready** — the specific gaps as a short list, each pointing at a file
  or an unanswered question. No padding.

Do not open the PR yourself unless the developer asks.
