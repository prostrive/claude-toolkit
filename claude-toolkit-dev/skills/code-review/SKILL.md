---
name: code-review
description: Provides professional code review context. Use when reviewing PRs, analyzing code quality, or checking work before shipping.
---

When reviewing code, apply these standards:

**Code Quality**
- Is the logic correct and does it handle edge cases?
- Is error handling present and meaningful (not silent failures)?
- Are there obvious performance issues (N+1 queries, missing indexes, blocking calls)?

**Maintainability**
- Would a new developer understand this in 6 months?
- Are functions/components doing one thing?
- Is there duplication that should be abstracted?

**Security**
- Is user input validated and sanitized?
- Are there hardcoded secrets or credentials?
- Is auth/authorization applied correctly?

**Production Readiness**
- Is the code consistent with the rest of the codebase style?
- Are there console.logs or debug artifacts left in?
- Does it match what was in the spec/PRD?

Flag issues as:
- **[BLOCKER]** — must fix before shipping
- **[WARNING]** — should fix, discuss with team
- **[SUGGESTION]** — nice to have, low priority
