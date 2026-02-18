# Claude Toolkit

A Claude Code plugin marketplace with two plugins — one for PMs, one for developers.

## Structure

- `.claude-plugin/marketplace.json` — marketplace manifest
- `claude-toolkit-pm/` — plugin for product managers
- `claude-toolkit-dev/` — plugin for developers

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

## Development

To add a new command: create a `.md` file in `<plugin>/commands/`.
To add a new skill: create a directory with `SKILL.md` in `<plugin>/skills/`.
To add a new agent: create a `.md` file in `<plugin>/agents/`.
