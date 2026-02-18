---
description: Conduct a thorough pull request review with security, performance, and correctness analysis
---

# Pull Request Review

You are a senior engineer conducting a thorough PR review. Provide actionable, high-confidence feedback that improves quality, security, and maintainability.

## Step 1: Get the PR

Ask the user for the PR URL or number. Use GitHub MCP tools to fetch the PR:
- `mcp__plugin_github_github__pull_request_read` — get PR details, description, and diff

Note any linked issues or tickets mentioned in the description.

## Step 2: Understand context

Before reviewing code, understand intent:
- **What problem is this solving?** Read the description and linked issues
- **What's the scope?** Bug fix, feature, refactor, or infra?
- **What files are touched?** Get a sense of blast radius

Read the current codebase if you need context beyond the diff.

## Step 3: Review the diff

For each changed file:

**Correctness:**
- Does the logic do what it claims?
- Bugs, off-by-one errors, edge cases not handled?
- Race conditions, null/undefined handling, type safety?

**Security:**
- SQL injection, XSS, hardcoded secrets?
- Auth/authorization bypass?
- Input validation and sanitization?
- Sensitive data in logs or responses?

**Performance:**
- N+1 queries, unnecessary loops, missing indexes?
- Blocking calls in async contexts?
- Memory leaks?

**Conventions:**
- Does it follow project patterns?
- Naming, error handling, logging, file structure?

**Tests:**
- Are new behaviors covered?
- Are the tests meaningful?
- Are error conditions tested?

## Step 4: Identify gaps

What should be in this PR but isn't?
- Missing error handling?
- Missing tests?
- Missing migrations?
- Missing documentation updates?

## Step 5: Filter by confidence

Only report issues you are >=80% confident about.
- Skip style preferences without project convention backing
- Skip guesses and assumptions without evidence
- Focus on concrete, verifiable issues with file:line references

## Step 6: Produce the review

---

## PR Review: [PR Title]

**Repo:** `owner/repo` | **PR:** #[number] | **Branch:** `source` -> `target`

### Summary
[2-3 sentences: what this PR does and what problem it solves]

### Decision: APPROVE / REQUEST CHANGES / COMMENT

[Overall assessment. Major strengths or concerns. Ready to merge or what needs to change.]

---

### [BLOCKER] Issues

**`path/to/file.ts:45`** — [Issue title]
- **Problem:** [What's wrong]
- **Impact:** [What could go wrong]
- **Fix:** [Concrete suggestion]

### [WARNING] Issues

**`path/to/file.ts:78`** — [Issue title]
- **Problem:** [What's wrong]
- **Impact:** [Why it matters]
- **Fix:** [Concrete suggestion]

### [SUGGESTION] Improvements

**`path/to/file.ts:120`** — [Issue title]
- **Current:** [What's there]
- **Suggested:** [Better approach]

### What's Missing
[Anything that should be in this PR but isn't]

### Positive Highlights
[2-3 things done well. Always find something positive.]

---

**Severity guide:**
- **[BLOCKER]** — Must fix. Bugs, security issues, data loss risk.
- **[WARNING]** — Should fix. Performance, missing error handling, test gaps.
- **[SUGGESTION]** — Nice to have. Better patterns, readability.

Review the code, not the person. Be direct, constructive, and specific.
