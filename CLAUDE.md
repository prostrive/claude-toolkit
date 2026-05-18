# Claude Toolkit

A Claude Code plugin marketplace. Currently registers `superpowers-lite` (a fork of [obra/superpowers](https://github.com/obra/superpowers) with a fast/full lane decision so small tasks skip the heavyweight planning ceremony). The `claude-toolkit-pm/` and `claude-toolkit-dev/` folders remain on disk as unregistered legacy and can be re-registered if needed.

## Structure

- `.claude-plugin/marketplace.json` — marketplace manifest
- `superpowers-lite/` — registered plugin (the active superpowers fork)
- `claude-toolkit-pm/` — unregistered legacy folder (commands for PRD generation, estimation, ticketing)
- `claude-toolkit-dev/` — unregistered legacy folder (commands for feature work, debugging, refactoring, etc., plus the 3 dev agents)

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

Fork of `obra/superpowers`. Same skills, same SessionStart-hook injection model, plus a **lane decision** in `using-superpowers` that classifies each task as fast or full. Full lane is bit-for-bit upstream behavior. Fast lane collapses ceremony for small, self-contained changes (no API/schema/infra change, contained blast radius).

## Development

To add a new command: create a `.md` file in `<plugin>/commands/`.
To add a new skill: create a directory with `SKILL.md` in `<plugin>/skills/`.
To add a new agent: create a `.md` file in `<plugin>/agents/`.
