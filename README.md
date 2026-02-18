# Claude Toolkit

A Claude Code plugin marketplace with two toolkits — one for PMs, one for developers.

## Installation

```bash
# Step 1: Add the marketplace
claude plugin marketplace add github:prostrive/claude-toolkit

# Step 2: Install the plugins you need
claude plugin install claude-toolkit-pm@claude-toolkit-marketplace
claude plugin install claude-toolkit-dev@claude-toolkit-marketplace
```

Then restart Claude Code. Commands will be available immediately.

## claude-toolkit-pm

Tools for product managers who can code a bit.

### Commands

| Command | Description |
|---------|-------------|
| `/generate-prd` | Analyze a codebase and generate a structured PRD — what exists, what's missing, what to build |
| `/estimate` | Produce a development effort estimate with breakdown by area |
| `/create-tickets` | Parse a PRD into Linear tickets via MCP with preview and approval |

### Skills

| Skill | Description |
|-------|-------------|
| `prd-generator` | Adds product thinking context when working with incomplete features or planning |

## claude-toolkit-dev

Tools for the builders.

### Commands

| Command | Description |
|---------|-------------|
| `/build-feature` | Guided feature development — explore, clarify, design, implement, review (7-phase workflow using all 3 agents) |
| `/fix-bug` | Systematic debugging — trace root cause, implement minimal fix, verify |
| `/refactor` | Analyze code smells, plan refactoring, execute safely with test verification |
| `/write-tests` | Generate meaningful tests matching project conventions and patterns |
| `/review-pr` | Thorough PR review — correctness, security, performance, conventions |
| `/audit-security` | Scan codebase for OWASP top 10, hardcoded secrets, auth gaps |
| `/generate-docs` | Generate technical documentation covering architecture, APIs, data models |
| `/onboard` | Generate a developer onboarding guide for new team members |
| `/setup-mcp` | Install standard MCP servers (Notion, Linear, GitHub, Playwright) globally |

### Skills

| Skill | Description |
|-------|-------------|
| `code-review` | Applies professional code review standards for production delivery |

### Agents

| Agent | Description |
|-------|-------------|
| `code-explorer` | Deep codebase analysis — traces execution paths, maps architecture |
| `code-architect` | Designs implementation blueprints aligned with existing codebase patterns |
| `code-reviewer` | Examines code for bugs, security issues, and convention violations |

## Adding new tools

- **Commands:** Add a `.md` file to `<plugin>/commands/`
- **Skills:** Add a directory with `SKILL.md` to `<plugin>/skills/`
- **Agents:** Add a `.md` file to `<plugin>/agents/`
