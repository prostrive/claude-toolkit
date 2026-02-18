---
description: Analyze the codebase and generate comprehensive technical documentation covering architecture, APIs, data models, and deployment
---

# Technical Documentation Generator

You are a senior developer writing technical documentation for a software project. Your goal is to produce docs that are accurate, scannable, and actually useful — not boilerplate.

## Step 1: Explore the codebase

- Read the README and any existing docs
- Map the folder structure and understand the architecture
- Identify the tech stack, frameworks, and key dependencies
- Read the main entry points, core modules, and key abstractions
- Find all API endpoints and understand their purpose
- Read the data models and understand relationships
- Check config files, env vars, and deployment setup

## Step 2: Write the documentation

Output in this format:

---

# Technical Documentation: [Project Name]

## Architecture Overview
Describe the high-level architecture. What are the main components, how do they communicate, and what does data flow look like? Include a simple ASCII diagram if it helps.

## Tech Stack
| Layer | Technology | Why |
|-------|-----------|-----|
| Frontend | ... | ... |
| Backend | ... | ... |
| Database | ... | ... |
| ... | | |

## Project Structure
Walk through the key directories. Focus on what matters, skip the obvious.

```
project/
├── src/
│   ├── [dir]   # what lives here
│   └── [dir]   # what lives here
```

## Data Models
Document the core entities, their fields, and relationships. Use a table format.

### [Model Name]
| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Primary key |
| ... | | |

**Relationships:** [describe how this model relates to others]

## API Reference
Document all endpoints. Group by resource.

### [Resource Name]

#### `METHOD /path`
**Description:** What this endpoint does

**Auth required:** Yes / No

**Request:**
```json
{
  "field": "type — description"
}
```

**Response:**
```json
{
  "field": "type — description"
}
```

**Errors:**
| Code | Meaning |
|------|---------|
| 400 | ... |
| 401 | ... |

---

## Environment Variables
| Variable | Required | Description | Example |
|----------|----------|-------------|---------|
| DATABASE_URL | Yes | PostgreSQL connection string | postgres://... |
| ... | | | |

## Key Flows
Walk through the 2-4 most important flows in the system end-to-end (e.g. user registration, loan origination, payment processing). For each:

### [Flow Name]
1. Step one — which module/file handles this
2. Step two — ...
3. ...

## External Integrations
| Service | Purpose | Docs |
|---------|---------|------|
| [Name] | What it does | [link if available] |

## Error Handling
How errors are handled, logged, and surfaced. Note any important conventions.

## Known Limitations & Tech Debt
Be honest about what's rough, what's temporary, and what will need revisiting.

---

Be precise. Reference actual file names and function names where relevant. Skip anything that's self-evident from the code. The reader is a developer who can read code — they need context and intent, not a description of what the code literally does.
