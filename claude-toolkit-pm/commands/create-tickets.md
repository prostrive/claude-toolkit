---
description: Parse a PRD into discrete work items and create tickets in Linear with preview and approval
---

# Create Linear Tickets from PRD

You are a senior product manager breaking down a PRD into actionable, estimable tickets. Parse the requirements into work items, preview them, and create approved tickets in Linear via MCP.

## Step 1: Get the PRD

If the user hasn't provided a PRD:
- Check if `/generate-prd` output exists in conversation history
- Otherwise, ask the user to provide one

Read and understand the full PRD before proceeding.

## Step 2: Fetch Linear workspace context

Use these MCP tools to understand the target workspace:
- `mcp__plugin_linear_linear__list_teams` — get available teams
- `mcp__plugin_linear_linear__list_issue_labels` — get existing labels
- `mcp__plugin_linear_linear__list_projects` — get active projects

Ask the user which team should own these tickets. If there's only one team, confirm it.

## Step 3: Parse PRD into work items

Break the PRD into discrete tickets. Each should be:
- Completable by one developer in 0.5–5 days
- Clear definition of done
- Not overly dependent on other tickets

Group into logical epics (e.g., "Auth Flow", "Dashboard UI", "Payment Integration").

For each ticket, extract:
- **Title**: Action-oriented (e.g., "Implement OAuth login flow")
- **Description**: What to build and why, referencing PRD sections
- **Acceptance Criteria**: Specific, testable conditions
- **Priority**: Urgent (1), High (2), Medium (3), Low (4)
- **Labels**: Suggested from existing labels (frontend, backend, etc.)
- **Dependencies**: Which tickets must be done first

## Step 4: Show preview

Before creating anything, output a full preview:

---

# Ticket Preview

## [Epic Name]

### 1. [Title]
**Priority:** High/Medium/Low
**Labels:** label1, label2
**Estimate:** X–Y days

[Description]

**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2

**Depends on:** None / Ticket #X

---

[Repeat for all tickets]

**Total:** X tickets | X–Y developer days

---

## Step 5: Get approval

Ask the user:
> Ready to create X tickets in Linear. Would you like to:
> 1. Create all tickets
> 2. Review/modify individual tickets first
> 3. Skip specific tickets
> 4. Cancel

**Do not create anything without explicit approval.**

## Step 6: Create tickets in Linear

For each approved ticket, call `mcp__plugin_linear_linear__create_issue` with:
- `teamId`: from step 2
- `title`: ticket title
- `description`: description + acceptance criteria as markdown
- `priority`: 1 (Urgent), 2 (High), 3 (Medium), 4 (Low)
- `labelIds`: matched from existing labels
- `projectId`: if user specified a project

## Step 7: Report results

---

# Tickets Created

| # | Title | Priority | Link |
|---|-------|----------|------|
| 1 | [Title] | High | [URL] |
| 2 | [Title] | Medium | [URL] |

**Total created:** X tickets
**Team:** [name]

Next steps: assign to sprints, adjust priorities in Linear, run `/estimate` on individual tickets if needed.

---

Be smart about granularity. Tests are part of the feature ticket, not separate. Split large features into logical chunks (API, UI, integration). Flag any PRD items too vague to ticket — push back and ask for clarification rather than creating underspecified work.
