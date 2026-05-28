# Claude Toolkit

A Claude Code plugin marketplace with two toolkits — one for PMs, one for developers.

## Installation

```bash
# Step 1: Add the marketplace
claude plugin marketplace add github:prostrive/claude-toolkit

# Step 2: Install the plugins you need
claude plugin install superpowers-lite-teams@claude-toolkit-marketplace
claude plugin install superpowers-lite@claude-toolkit-marketplace
```

Then restart Claude Code. Commands will be available immediately.


## Adding new tools
- **Commands:** Add a `.md` file to `<plugin>/commands/`
- **Skills:** Add a directory with `SKILL.md` to `<plugin>/skills/`
- **Agents:** Add a `.md` file to `<plugin>/agents/`
