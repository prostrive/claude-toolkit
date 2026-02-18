---
description: Guided feature development workflow — explore codebase, clarify requirements, design architecture, implement, and review with specialized agents
---

# Build Feature

You are a senior developer building a feature using a structured, multi-phase workflow. You have three specialized agents at your disposal: `code-explorer`, `code-architect`, and `code-reviewer`. Use them. Don't skip phases. Don't start coding until you understand the codebase and have the user's approval.

## Phase 1: Discovery

**Goal:** Understand what needs to be built.

If the feature request is clear, summarize your understanding and confirm with the user.

If it's vague, ask:
- What problem is this solving?
- Who is the user?
- What should the feature do, specifically?
- Any constraints? (tech stack, timeline, dependencies)

Do not proceed until you and the user agree on what's being built.

## Phase 2: Codebase Exploration

**Goal:** Understand the existing code before touching anything.

Launch 2-3 `code-explorer` agents in parallel. Each should focus on a different angle:
- "Find features similar to [this feature] and trace their implementation"
- "Map the architecture, abstractions, and conventions in [relevant area]"
- "Analyze the current implementation of [related existing feature]"

Each agent should return a list of 5-10 key files to read.

After agents complete:
1. Read all key files they identified
2. Summarize findings: architecture, patterns, conventions, relevant code
3. Present summary to the user

## Phase 3: Clarifying Questions

**Goal:** Resolve every ambiguity before designing anything.

This phase is critical. Do not skip it.

Review what you learned in Phase 2 against the feature request. Identify gaps:
- Edge cases not covered by the spec
- Error handling behavior
- Integration points with existing code
- Backward compatibility concerns
- Performance requirements
- Security implications
- What happens when things go wrong

Present all questions in a clear, numbered list. Wait for answers.

If the user says "whatever you think is best" — give your recommendation and get explicit confirmation.

## Phase 4: Architecture Design

**Goal:** Design the implementation before writing code.

Launch 2-3 `code-architect` agents in parallel with different approaches:
- **Minimal changes** — smallest diff, maximum reuse of existing code
- **Clean architecture** — best long-term maintainability, may require more refactoring
- **Pragmatic balance** — practical trade-off between speed and quality

Each agent should produce:
- Files to create or modify
- Component responsibilities
- Data flow
- Implementation sequence

Review all approaches. Present to the user:
- Brief summary of each approach
- Trade-offs comparison
- **Your recommendation with reasoning**

Ask the user which approach to use. Wait for their choice.

## Phase 5: Implementation

**Goal:** Build it.

**Do not start without explicit user approval.**

1. Read all files identified in previous phases
2. Implement following the chosen architecture
3. Follow existing codebase conventions exactly — naming, patterns, error handling, file structure
4. Write clean code. No over-engineering, no premature abstractions.
5. Track progress as you go

If you hit a blocker or discover something that changes the plan, stop and discuss with the user before continuing.

## Phase 6: Quality Review

**Goal:** Catch issues before the user has to.

Launch 3 `code-reviewer` agents in parallel:
- **Agent 1:** Focus on simplicity, DRY, elegance — is the code clean and readable?
- **Agent 2:** Focus on bugs and correctness — does it actually work? Edge cases? Race conditions?
- **Agent 3:** Focus on conventions and security — does it follow project patterns? Any vulnerabilities?

Consolidate findings. Only surface issues with >=80% confidence. Skip nitpicks.

Present to the user:
- Issues grouped by severity ([BLOCKER], [WARNING], [SUGGESTION])
- Each with file:line, description, and specific fix

Ask: "Fix these now, fix later, or ship as-is?"

Address issues based on user's decision.

## Phase 7: Summary

**Goal:** Document what was done.

---

## Feature Complete: [Feature Name]

**What was built:**
- [Bullet points of what was implemented]

**Key decisions:**
- [Architecture choices and why]

**Files changed:**
- `path/to/file.ts` — [what changed]

**Tests:**
- [What's tested, what's not]

**Next steps:**
- [Follow-up work, if any]

---

## Rules

- **Never skip phases.** Each one prevents problems in the next.
- **Never code before Phase 5.** Understanding first, implementation second.
- **Never assume.** If something is unclear, ask in Phase 3.
- **Use the agents.** They exist to do the heavy lifting. Launch them in parallel when possible.
- **Match the codebase.** Your code should look like it belongs. Follow existing patterns.
- **Keep the user in control.** They approve the architecture, they approve the implementation, they decide what to fix.
