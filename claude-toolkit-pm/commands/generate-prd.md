---
description: Analyze the codebase and generate a structured PRD identifying what exists, what's missing, and what needs to be built
---

# Generate PRD from Codebase Analysis

You are a senior product manager analyzing a codebase to understand what exists, what's missing, and what needs to be built. Your job is to produce a clear, actionable PRD.

## Step 1: Explore the codebase

- Read the README, package.json/requirements.txt, and config files at the root
- Map out the folder structure
- Identify the tech stack, frameworks, and key dependencies
- Read the main entry points and core modules

## Step 2: Identify what exists

Document what is already built:
- Features and flows that appear complete
- API endpoints or routes that are implemented
- UI screens or components that exist
- Integrations that are in place
- Data models and their relationships

## Step 3: Identify what's missing or incomplete

Look for signals of incomplete work:
- `TODO`, `FIXME`, `HACK`, `WIP` comments
- Placeholder functions, empty implementations, or `throw new Error('not implemented')`
- Routes or components defined but not connected
- Referenced files or modules that don't exist
- Features mentioned in comments but not built
- Mismatches between frontend and backend (UI expects an API that doesn't exist)
- Empty or skipped tests hinting at untested functionality
- Dead-end navigation flows (screens that lead nowhere)
- Hardcoded dummy data where real data fetching should be

## Step 4: Infer the product vision

- What is this product trying to do?
- Who is the target user?
- What does the core user journey look like end-to-end?
- What is MVP completion vs. nice-to-haves?

## Step 5: Write the PRD

Output in this exact format:

---

# PRD: [Product Name]

## Overview
Brief description of what this product does and who it's for.

## Current State
What is already built and working. Be specific — reference actual files or features.

## Gaps & Missing Functionality
What is incomplete or missing, grouped by area (e.g. Auth, Payments, Dashboard).

For each gap:
- **What's missing**: description
- **Evidence**: where in the code you found this signal
- **Priority**: High / Medium / Low

## User Stories
For each major gap, write:
> As a [user], I want to [action] so that [outcome].

Include acceptance criteria for each.

## Technical Notes
Architectural observations the dev team should be aware of. Flag tech debt or risky patterns.

## Suggested Build Order
Recommended sequence to complete the product, respecting dependencies.

## Out of Scope
Things clearly not part of the current vision that should not be built now.

---

Be thorough but practical. The goal is to give the development team a clear picture of exactly what needs to be built to reach a shippable product.
