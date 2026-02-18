---
description: Install standard MCP servers (Notion, Linear, GitHub, Playwright) globally so they're available in every project
---

# Setup MCP Servers

Install standard MCP servers globally on this device. This makes them available in every project and every session — no per-project config needed.

## Run these commands

Execute each of these commands using the Bash tool:

```bash
claude mcp add --transport http notion https://mcp.notion.com/mcp --scope user
```

```bash
claude mcp add --transport http linear https://mcp.linear.app/mcp --scope user
```

```bash
claude mcp add --transport http github https://api.githubcopilot.com/mcp/ --scope user
```

```bash
claude mcp add --transport stdio playwright --scope user -- npx -y @playwright/mcp@latest
```

## After running

1. Tell the user to restart Claude Code for changes to take effect
2. Tell them to run `/mcp` after restarting to verify all four servers appear
3. Notion, Linear, and GitHub will require OAuth authentication on first use — they'll be prompted automatically
4. Playwright works immediately with no auth
