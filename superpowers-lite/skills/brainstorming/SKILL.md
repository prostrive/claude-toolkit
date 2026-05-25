---
name: brainstorming
description: "You MUST use this before any creative work - creating features, building components, adding functionality, or modifying behavior. Explores user intent, requirements and design before implementation. Branches on the fast/middle/full lane from using-superpowers."
---

# Brainstorming Ideas Into Designs

Help turn ideas into fully formed designs and specs through natural collaborative dialogue.

This skill has **three lane modes** controlled by the lane decision from `using-superpowers`:

- **Full lane**: the full design-first workflow below — one question at a time, design sections with per-section approval, committed spec doc, spec self-review, user review gate.
- **Middle lane**: **skip this skill entirely.** Middle-lane tasks slot into existing patterns and have no design fork to argue. Go directly to `writing-plans` (its Middle-Lane Short Plan path will handle any clarifying questions). If a real design fork surfaces mid-work — two viable architectures with different long-term implications, a new dependency, new infrastructure — STOP and ask the user to reclassify to full lane.
- **Fast lane**: the **Fast-Lane Mini-Brainstorm** at the bottom of this skill — one consolidated message with any clarifying questions and a proposed approach, inline approval, no spec doc.

If `using-superpowers` did not set a lane, return to `using-superpowers` and have the user pick. Do not pick yourself.

<HARD-GATE>
**Full lane:** Do NOT invoke any implementation skill, write any code, scaffold any project, or take any implementation action until you have presented a design and the user has approved it. This applies to EVERY full-lane project regardless of perceived simplicity.

**Middle lane:** Do not run brainstorming. Invoke `writing-plans` directly.

**Fast lane:** Run the Fast-Lane Mini-Brainstorm. Implementation begins after a single inline approval from the user. No committed spec doc.
</HARD-GATE>

## Anti-Pattern: "This Is Too Simple To Need A Design"

**Full lane:** Every project goes through this process. A todo list, a single-function utility, a config change — all of them. "Simple" projects are where unexamined assumptions cause the most wasted work. The design can be short (a few sentences for truly simple projects), but you MUST present it and get approval.

**Middle lane:** This anti-pattern does NOT apply — the user picked middle lane because the task slots into an existing pattern and there's no design judgment to make. The plan doc is the artifact; the design is the existing system. If you find yourself wanting to brainstorm, that's a signal a design fork has surfaced — stop and ask the user to reclassify to full lane.

**Fast lane:** This anti-pattern does NOT apply — the fast lane exists precisely because for genuinely small, self-contained tasks (no API/schema change, no new infra, contained blast radius), the design ceremony costs more than it saves. If during fast-lane brainstorming you discover hidden complexity (new dependency, schema change, multi-file blast radius beyond the contained area), STOP and ask the user to reclassify.

## Checklist

You MUST create a task for each of these items and complete them in order:

1. **Explore project context** — check files, docs, recent commits
2. **Offer visual companion** (if topic will involve visual questions) — this is its own message, not combined with a clarifying question. See the Visual Companion section below.
3. **Ask clarifying questions** — one at a time, understand purpose/constraints/success criteria
4. **Propose relevant approaches** — with trade-offs and your recommendation
5. **Present design** — in sections scaled to their complexity, get user approval after each section
6. **Write design doc** — save to `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md` and commit
7. **Spec self-review** — quick inline check for placeholders, contradictions, ambiguity, scope (see below)
8. **User reviews written spec** — ask user to review the spec file before proceeding
9. **Transition to implementation** — invoke writing-plans skill to create implementation plan

**The terminal state is invoking writing-plans.** Do NOT invoke frontend-design, mcp-builder, or any other implementation skill. The ONLY skill you invoke after brainstorming is writing-plans.

## The Process

**Understanding the idea:**

- Check out the current project state first (files, docs, recent commits)
- Before asking detailed questions, assess scope: if the request describes multiple independent subsystems (e.g., "build a platform with chat, file storage, billing, and analytics"), flag this immediately. Don't spend questions refining details of a project that needs to be decomposed first.
- If the project is too large for a single spec, help the user decompose into sub-projects: what are the independent pieces, how do they relate, what order should they be built? Then brainstorm the first sub-project through the normal design flow. Each sub-project gets its own spec → plan → implementation cycle.
- For appropriately-scoped projects, ask questions one at a time to refine the idea
- Prefer multiple choice questions when possible, but open-ended is fine too
- Only one question per message - if a topic needs more exploration, break it into multiple questions
- Focus on understanding: purpose, constraints, success criteria

**Exploring approaches:**

- Propose the relevant approaches with trade-offs (don't force a fixed count — surface only the options that are genuinely worth considering)
- Present options conversationally with your recommendation and reasoning
- Lead with your recommended option and explain why

**Presenting the design:**

- Once you believe you understand what you're building, present the design
- Scale each section to its complexity: a few sentences if straightforward, up to 200-300 words if nuanced
- Ask after each section whether it looks right so far
- Cover: architecture, components, data flow, error handling, testing
- Be ready to go back and clarify if something doesn't make sense

**Design for isolation and clarity:**

- Break the system into smaller units that each have one clear purpose, communicate through well-defined interfaces, and can be understood and tested independently
- For each unit, you should be able to answer: what does it do, how do you use it, and what does it depend on?
- Can someone understand what a unit does without reading its internals? Can you change the internals without breaking consumers? If not, the boundaries need work.
- Smaller, well-bounded units are also easier for you to work with - you reason better about code you can hold in context at once, and your edits are more reliable when files are focused. When a file grows large, that's often a signal that it's doing too much.

**Working in existing codebases:**

- Explore the current structure before proposing changes. Follow existing patterns.
- Where existing code has problems that affect the work (e.g., a file that's grown too large, unclear boundaries, tangled responsibilities), include targeted improvements as part of the design - the way a good developer improves code they're working in.
- Don't propose unrelated refactoring. Stay focused on what serves the current goal.

## After the Design

**Documentation:**

- Write the validated design (spec) to `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`
  - (User preferences for spec location override this default)
- Use elements-of-style:writing-clearly-and-concisely skill if available
- Commit the design document to git

**Spec Self-Review:**
After writing the spec document, look at it with fresh eyes:

1. **Placeholder scan:** Any "TBD", "TODO", incomplete sections, or vague requirements? Fix them.
2. **Internal consistency:** Do any sections contradict each other? Does the architecture match the feature descriptions?
3. **Scope check:** Is this focused enough for a single implementation plan, or does it need decomposition?
4. **Ambiguity check:** Could any requirement be interpreted two different ways? If so, pick one and make it explicit.

Fix any issues inline. No need to re-review — just fix and move on.

**User Review Gate:**
After the spec review loop passes, ask the user to review the written spec before proceeding:

> "Spec written and committed to `<path>`. Please review it and let me know if you want to make any changes before we start writing out the implementation plan."

Wait for the user's response. If they request changes, make them and re-run the spec review loop. Only proceed once the user approves.

**Implementation:**

- Invoke the writing-plans skill to create a detailed implementation plan
- Do NOT invoke any other skill. writing-plans is the next step.

## Key Principles

- **One question at a time** - Don't overwhelm with multiple questions
- **Multiple choice preferred** - Easier to answer than open-ended when possible
- **YAGNI ruthlessly** - Remove unnecessary features from all designs
- **Explore alternatives** - Surface the relevant approaches before settling (no fixed count)
- **Incremental validation** - Present design, get approval before moving on
- **Be flexible** - Go back and clarify when something doesn't make sense

## Visual Companion

A browser-based companion for showing mockups, diagrams, and visual options during brainstorming. Available as a tool — not a mode. Accepting the companion means it's available for questions that benefit from visual treatment; it does NOT mean every question goes through the browser.

**Offering the companion:** When you anticipate that upcoming questions will involve visual content (mockups, layouts, diagrams), offer it once for consent:
> "Some of what we're working on might be easier to explain if I can show it to you in a web browser. I can put together mockups, diagrams, comparisons, and other visuals as we go. This feature is still new and can be token-intensive. Want to try it? (Requires opening a local URL)"

**This offer MUST be its own message.** Do not combine it with clarifying questions, context summaries, or any other content. The message should contain ONLY the offer above and nothing else. Wait for the user's response before continuing. If they decline, proceed with text-only brainstorming.

**Per-question decision:** Even after the user accepts, decide FOR EACH QUESTION whether to use the browser or the terminal. The test: **would the user understand this better by seeing it than reading it?**

- **Use the browser** for content that IS visual — mockups, wireframes, layout comparisons, architecture diagrams, side-by-side visual designs
- **Use the terminal** for content that is text — requirements questions, conceptual choices, tradeoff lists, A/B/C/D text options, scope decisions

A question about a UI topic is not automatically a visual question. "What does personality mean in this context?" is a conceptual question — use the terminal. "Which wizard layout works better?" is a visual question — use the browser.

If they agree to the companion, read the detailed guide before proceeding:
`skills/brainstorming/visual-companion.md`

---

## Fast-Lane Mini-Brainstorm (superpowers-lite)

Use this when `using-superpowers` selected the fast lane. The goal is to get to "approved, start coding" in **one message exchange**, not five.

### Process

1. **Quick context scan** — read the file(s) the task is about. Skip the broad project-wide exploration; the change is local, so the context that matters is local.

2. **Produce one consolidated message** with up to three parts:
   - **(a) Clarifying questions** — only if genuinely needed. Batch them together (multiple choice preferred). If you don't have meaningful uncertainty, skip this part entirely.
   - **(b) Proposed approach** — 2–6 sentences. Files you'll touch, what changes, the test you'll write. Skip alternative-approach trade-offs unless there's a real fork in the road.
   - **(c) Approval ask** — "Sound good? I'll proceed." or equivalent.

3. **On approval** — go straight to implementation. Do not invoke `writing-plans` for a separate plan doc. Use TodoWrite in-conversation if you need a checklist (see writing-plans fast-lane section), then start coding.

4. **No spec file. No self-review pass. No separate user-review gate.** Those exist in the full lane to protect against drift on multi-day work. They are not load-bearing on a one-file change.

### When to bail out of the fast lane

If during the mini-brainstorm you discover any of these, STOP and ask the user to reclassify (do not pick yourself):

- The change actually touches more files than you thought → likely middle lane
- There's a real design fork (e.g., two architectures with different long-term implications) → full lane
- You need new infrastructure / a new dependency → full lane
- A schema change surfaces — pre-launch this is middle lane, post-launch it's full lane
- The user asks for a written spec or design doc → full lane

Switching lanes is cheap; over-committing to fast lane is expensive. Surface the new scope to the user via AskUserQuestion ("This is bigger than fast lane — should we switch to middle or full?") and let them pick.

### What stays mandatory in the fast lane

- **Read the code first.** Don't propose changes blind.
- **Write a test** (see `test-driven-development` — fast-lane behavior). The fast lane never means "no tests."
- **Commit after the change passes.** Frequent commits are not optional.
- **Verification before completion.** Run the test, run the lint, confirm the thing actually works.

The fast lane removes ceremony, not discipline.
