---
description: Explore the codebase and generate a developer onboarding guide so new team members can get up to speed fast
---

# Developer Onboarding Guide Generator

You are a senior developer who just finished exploring a codebase. Your job is to write a clear onboarding guide for a new developer joining this project.

## Step 1: Explore the project

- Read README, setup docs, and any existing documentation
- Map the folder structure and understand what each major directory does
- Identify the tech stack, key frameworks, and important libraries
- Find the main entry points and understand how the app boots
- Look at the data models and understand the core domain
- Find the test setup and understand how to run tests
- Check CI/CD config (.github/workflows, etc.)

## Step 2: Identify common gotchas

Look for:
- Non-obvious environment setup requirements
- Services that need to be running locally (databases, queues, etc.)
- Env variables that aren't documented
- Parts of the codebase that are particularly complex or tricky
- Known issues or TODOs that a new dev might trip over

## Step 3: Write the guide

---

# Developer Onboarding: [Project Name]

## What is this?
One paragraph: what does this product do and who uses it.

## Tech Stack
Quick list of the main technologies and why they're used.

## Getting Started

### Prerequisites
What needs to be installed before anything works.

### Setup Steps
Step-by-step from clone to running locally. Be specific — include exact commands.

### Environment Variables
List required env vars, what they're for, and where to get them.

## Project Structure
Walk through the key directories and what lives where. Focus on what a developer will actually touch.

## Key Concepts
Explain 3-5 domain concepts or architectural decisions a new dev needs to understand to be effective.

## Common Tasks
How to do the things developers do most:
- Run the app locally
- Run tests
- Add a new API endpoint / UI page
- Deploy (if relevant)

## Gotchas & Known Issues
The stuff that isn't in the docs but will bite you.

## Who to Ask
(Leave blank — team to fill in)

---

Write this for a developer who is technically strong but has never seen this codebase. Be specific and practical. Skip the obvious.
