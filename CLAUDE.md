# Claude Toolkit

A Claude Code plugin marketplace with three plugins — one for PMs, one for developers, and a lighter fork of the superpowers methodology.

## Structure

- `.claude-plugin/marketplace.json` — marketplace manifest
- `claude-toolkit-pm/` — plugin for product managers
- `claude-toolkit-dev/` — plugin for developers
- `superpowers-lite/` — fork of [obra/superpowers](https://github.com/obra/superpowers) with a fast/full lane decision so small tasks skip the heavyweight planning ceremony (see `superpowers-lite/CHANGES.md`)

## Plugin: claude-toolkit-pm

### Commands
- `/generate-prd` — Analyze a codebase and generate a structured PRD
- `/estimate` — Produce a development effort estimate with breakdown
- `/create-tickets` — Parse a PRD into work items and create tickets in Linear

### Skills
- `prd-generator` — Adds product thinking context for feature planning and gap analysis

## Plugin: claude-toolkit-dev

### Commands
- `/build-feature` — Guided feature development: explore, clarify, design, implement, review (uses all 3 agents)
- `/fix-bug` — Systematic debugging: trace root cause, implement fix, verify
- `/refactor` — Analyze code smells, plan refactoring, execute safely
- `/write-tests` — Generate meaningful tests matching project conventions
- `/review-pr` — Thorough PR review with confidence-based filtering
- `/audit-security` — Scan codebase for security vulnerabilities
- `/generate-docs` — Generate comprehensive technical documentation
- `/onboard` — Generate a developer onboarding guide
- `/setup-mcp` — Install standard MCP servers globally

### Skills
- `code-review` — Professional code review standards for production delivery

### Agents
- `code-explorer` — Deep codebase analysis, traces execution paths and maps architecture
- `code-architect` — Designs implementation blueprints aligned with existing patterns
- `code-reviewer` — Examines code for bugs, security issues, and convention violations

## Plugin: superpowers-lite

Fork of `obra/superpowers` v5.1.0. Same 14 skills, same SessionStart-hook injection model, plus a **lane decision** in `using-superpowers` that classifies each task as fast or full. Full lane is bit-for-bit upstream behavior. Fast lane collapses ceremony for tasks that touch ≤2 files with no API/schema change.

See `superpowers-lite/CHANGES.md` for the exact divergence and merge strategy for future upstream updates.

## Development

To add a new command: create a `.md` file in `<plugin>/commands/`.
To add a new skill: create a directory with `SKILL.md` in `<plugin>/skills/`.
To add a new agent: create a `.md` file in `<plugin>/agents/`.
