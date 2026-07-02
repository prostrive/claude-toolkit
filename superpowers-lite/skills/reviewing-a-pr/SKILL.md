---
name: reviewing-a-pr
description: Use when you want to understand a specific PR before reviewing it — paste a PR URL and ask questions about it (why is this here, is it consistent with the prior/reference codebase, does it break conventions). Loads the PR + the repo's conventions + a reference codebase, then answers conversationally and fast. Runs best on Sonnet 5.
---

# Reviewing a PR

A conversational companion for getting your head around one PR, fast. You paste a PR URL and ask questions; it answers grounded in the diff, the repo's conventions, and (if there is one) a prior/reference version of the codebase.

This is **not** a full adversarial review — for that, use `requesting-code-review` (dispatches a reviewer) or the `code-reviewer` agent. This is the "help me understand this change so I can review it" tool.

**Run it on Sonnet 5.** It's meant to be snappy — quick lookups, not deep reasoning. If you're on a heavier model, say so once: _"This is faster on Sonnet 5 — `/model` to switch."_ Escalate only for a single question that genuinely needs cross-codebase reasoning (usually a parity one), and say you're doing it first.

**Announce at start:** "I'm using the reviewing-a-pr skill — paste the PR URL and ask away."

## Step 1: Load the PR

```bash
gh pr view <url> --json title,body,files,commits
gh pr diff <url>
```

Read in this order:

1. **The PR body** — if it has a `## Rundown` section, that's the author's own mental model of the change; start there, it answers most "why is this here" questions.
2. **The diff** — what actually changed.
3. **The commit messages** — often carry the "why" the diff doesn't.

Then give a two-line orientation ("this is the tenant-config write path; ~6 files; the meat's in the service + the consumer") and ask: "What do you want to know?"

## Step 2: Ground each question in the right place — and cite it

The questions people actually ask map to three sources. Go to the right one, and name what you looked at:

- **"Why is this here?"** → the Rundown / PR body / commit messages first; if they don't say, read the code _around_ the change to infer it. If it's still not clear, **say so** — don't invent a reason. "The PR doesn't explain this; you'd have to ask the author" beats a confident guess.
- **"Is this in line with the old version / v2?"** → the reference codebase, if there is one (for EBANQ that's the `v2` checkout — usually a sibling of this repo, e.g. `../v2`; ask for the path if it's not there). Find the equivalent, compare behavior, name the file. This is the one worth **escalating off Sonnet** if it needs real cross-codebase reasoning — flag it first. If you can't reach the reference codebase, say "I can't see v2 from here" — don't guess parity.
- **"Does this break conventions?"** → `AGENTS.md` and `docs/decision-records/`. Quote the specific rule it breaks, or confirm it follows one. "Feels off" isn't an answer — cite the convention, or say there isn't one.

## Style

- **Fast and concrete.** Answer the question asked; cite `file:line` and the specific convention or reference file. Don't pad, don't hedge, don't restate the diff back at them.
- **"I can't tell" beats a guess** — especially on "why" and parity. Say what you'd need to know for sure.
- **Stay a companion, not an auditor.** Don't drift into a line-by-line review unless asked — point at `requesting-code-review` for that.
