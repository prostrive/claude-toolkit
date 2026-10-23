# Claude Toolkit

A Claude Code plugin marketplace. Registers `superpowers-lite` (a fork of [obra/superpowers](https://github.com/obra/superpowers) with a developer-picked fast/middle/full lane so each task gets right-sized planning ceremony) and `superpowers-lite-teams` (the same plugin with Agent Teams parallel execution enabled by default on the full lane).

## Structure

- `.claude-plugin/marketplace.json` — marketplace manifest
- `superpowers-lite/` — registered plugin (the active superpowers fork)
- `superpowers-lite-teams/` — registered plugin (superpowers-lite + Agent Teams on the full lane, ports [narwhalishus/superpowered-teams](https://github.com/narwhalishus/superpowered-teams))


## Plugin: superpowers-lite

Fork of `obra/superpowers`. Same skills, same SessionStart-hook injection model, plus a **three-lane decision** in `using-superpowers`. The developer picks the lane via AskUserQuestion at the start of each task — the AI does not classify:

- **Full lane** — bit-for-bit upstream behavior: design-first brainstorm + spec doc + full plan doc + per-task two-stage subagent review. Use for new dependencies, new infra, real architectural decisions, post-launch breaking changes, RFC-class work.
- **Middle lane** — short committed plan doc, no brainstorm/spec, optional implementer subagent, single combined reviewer pass at end. Use for medium refactors, multi-file bug fixes, features that slot into existing patterns, internal API changes, dep upgrades, perf fixes, pre-launch schema/API changes.
- **Fast lane** — mini-brainstorm, TodoWrite plan, inline implementation, no subagents. Use for ≤~4-file self-contained changes (copy, styling, single-function fix, config flip).

All three lanes keep TDD and verification-before-completion mandatory. Lane reclassification mid-task goes back through AskUserQuestion.

## Plugin: superpowers-lite-teams

Same as `superpowers-lite`, with two skills and one agent ported from [narwhalishus/superpowered-teams](https://github.com/narwhalishus/superpowered-teams) (MIT) and wired into the full lane by default.

### What's added on top of superpowers-lite

- **Skill `writing-plans-for-teams`** — runs a four-criterion fitness check on the spec (≥4 tasks, ≥2 waves with ≥2 tasks each, ≥2 distinct specialist roles, no pervasive shared state). On pass, writes a wave-structured team plan. On fail, hands off to `writing-plans` for serial flow.
- **Skill `agent-team-driven-development`** — executes team-format plans by orchestrating 1–3 persistent specialist teammates in parallel waves, with two-stage review (spec compliance, then code quality) per task via one-shot reviewer subagents.
- **Agent `code-reviewer`** — used by the team flow for code-quality and final cross-cutting review.

### How it's wired

The full-lane brainstorming terminal in this plugin's `brainstorming` skill invokes `writing-plans-for-teams` instead of `writing-plans`. The fitness check then either produces a team plan or falls back to the standard serial flow. Middle and fast lanes are unchanged.

### Prerequisites

- Claude Code ≥ 2.1.32
- `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` in `~/.claude/settings.json`

Without the flag, the fitness check still runs and the serial fallback path works; only team execution itself requires the flag.

### Status

Experimental — kept side-by-side with `superpowers-lite` so the teams behavior can be tested without disrupting non-teams workflows. Don't install both into the same Claude Code instance; pick one.

## Development

To add a new command: create a `.md` file in `<plugin>/commands/`.
To add a new skill: create a directory with `SKILL.md` in `<plugin>/skills/`.
To add a new agent: create a `.md` file in `<plugin>/agents/`.
