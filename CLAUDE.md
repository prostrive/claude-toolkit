# Claude Toolkit

A Claude Code plugin marketplace. Currently registers `superpowers-lite` (a fork of [obra/superpowers](https://github.com/obra/superpowers) with a developer-picked fast/middle/full lane so each task gets right-sized planning ceremony). The `claude-toolkit-pm/` and `claude-toolkit-dev/` folders remain on disk as unregistered legacy and can be re-registered if needed.

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

Fork of `obra/superpowers`. Same skills, same SessionStart-hook injection model, plus a **three-lane decision** in `using-superpowers`. The developer picks the lane via AskUserQuestion at the start of each task — the AI does not classify:

- **Full lane** — bit-for-bit upstream behavior: design-first brainstorm + spec doc + full plan doc + per-task two-stage subagent review. Use for new dependencies, new infra, real architectural decisions, post-launch breaking changes, RFC-class work.
- **Middle lane** — short committed plan doc, no brainstorm/spec, optional implementer subagent, single combined reviewer pass at end. Use for medium refactors, multi-file bug fixes, features that slot into existing patterns, internal API changes, dep upgrades, perf fixes, pre-launch schema/API changes.
- **Fast lane** — mini-brainstorm, TodoWrite plan, inline implementation, no subagents. Use for ≤~4-file self-contained changes (copy, styling, single-function fix, config flip).

All three lanes keep TDD and verification-before-completion mandatory. Lane reclassification mid-task goes back through AskUserQuestion.

## Development

To add a new command: create a `.md` file in `<plugin>/commands/`.
To add a new skill: create a directory with `SKILL.md` in `<plugin>/skills/`.
To add a new agent: create a `.md` file in `<plugin>/agents/`.
