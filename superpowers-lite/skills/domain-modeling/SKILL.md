---
name: domain-modeling
description: Use when a domain term is contested, fuzzy, overloaded, or newly introduced — or when a hard-to-reverse decision needs recording. Maintains the repo's committed domain glossary (CONTEXT.md) and decision records (docs/adr/). Every lane READS CONTEXT.md; only this skill CHANGES it.
---

# Domain Modeling

Build and sharpen the project's domain language, and write it down the moment it crystallises.

Ported from [mattpocock/skills](https://github.com/mattpocock/skills) (MIT), adapted for the superpowers-lite lane model.

## Reading vs. maintaining — know which one you're doing

These are different, and conflating them is the main way this skill gets misused.

**Reading `CONTEXT.md` is a one-line habit, not this skill.** Every lane does it. Before your first substantive action on a task, if `CONTEXT.md` (or `CONTEXT-MAP.md`) exists, read it. Then use its canonical terms — in code, identifiers, comments, commit messages, spec docs, plan docs, and conversation with the user. **Never use a term the glossary lists under `_Avoid_`.** That costs one file read and no ceremony.

**This skill is for when you are CHANGING the model** — challenging a term, sharpening a fuzzy one, resolving a naming conflict, or recording a decision. Invoke it then, and not otherwise.

<HARD-GATE>
Do NOT invoke this skill just to look up vocabulary. Read the file.
Do NOT write to `CONTEXT.md` without invoking this skill.
</HARD-GATE>

## Lane Branch (superpowers-lite)

**Every lane — read.** As above. One file read, no invocation.

**Full lane — invoke during design.** When brainstorming surfaces a new domain concept, a contested term, or two words for the same thing, invoke this skill *before* the spec doc is written, so the spec is written in canonical vocabulary. Offer an ADR at genuine design forks (see the three tests below).

**Middle lane — invoke only if the model changes.** Most middle-lane work slots into existing patterns and introduces no new vocabulary; read `CONTEXT.md` and move on. Invoke this skill only when the change *introduces or renames a domain concept* — a new entity, a new state in a lifecycle, a rename that will show up in identifiers.

**Fast lane — read only. Do not invoke.** A ≤4-file change should not be minting domain vocabulary. If you find yourself wanting to define a new term during fast-lane work, that is a signal the task is bigger than fast lane — stop and ask the user to reclassify to middle (see `using-superpowers`). Do not define the term and carry on.

## File structure

Most repos have a single context:

```
/
├── CONTEXT.md
├── docs/
│   └── adr/
│       ├── 0001-event-sourced-orders.md
│       └── 0002-postgres-for-write-model.md
└── src/
```

If a `CONTEXT-MAP.md` exists at the root, the repo has multiple contexts. The map points to where each one lives:

```
/
├── CONTEXT-MAP.md
├── docs/
│   └── adr/                          ← system-wide decisions
├── src/
│   ├── ordering/
│   │   ├── CONTEXT.md
│   │   └── docs/adr/                 ← context-specific decisions
│   └── billing/
│       ├── CONTEXT.md
│       └── docs/adr/
```

Create files lazily — only when you have something to write. If no `CONTEXT.md` exists, create one when the first term is resolved. If no `docs/adr/` exists, create it when the first ADR is needed.

These live alongside superpowers' own artifacts, they don't replace them. `docs/superpowers/specs/` holds *designs* (what we're building). `CONTEXT.md` holds *vocabulary* (what the words mean). `docs/adr/` holds *decisions* (why we chose this). A spec goes stale when it ships; a glossary and an ADR don't.

## Do not seed the glossary from a codebase scan

The single worst failure mode for this skill is generating a 40-term `CONTEXT.md` by reading `src/` and listing every noun. That produces a document nobody trusts and nobody maintains.

**Every entry must come from a resolved ambiguity** — a moment where two words meant one thing, or one word meant two things, and a human picked. If nothing was contested, there's nothing to write. A `CONTEXT.md` with four hard-won terms is worth more than one with forty inferred ones.

## During the session

### Challenge against the glossary

When the user uses a term that conflicts with the existing language in `CONTEXT.md`, call it out immediately. "Your glossary defines 'cancellation' as X, but you seem to mean Y — which is it?"

### Sharpen fuzzy language

When the user uses vague or overloaded terms, propose a precise canonical term. "You're saying 'account' — do you mean the Customer or the User? Those are different things."

### Discuss concrete scenarios

When domain relationships are being discussed, stress-test them with specific scenarios. Invent scenarios that probe edge cases and force the user to be precise about the boundaries between concepts.

### Cross-reference with code

When the user states how something works, check whether the code agrees. If you find a contradiction, surface it: "Your code cancels entire Orders, but you just said partial cancellation is possible — which is right?"

### Update CONTEXT.md inline

When a term is resolved, update `CONTEXT.md` right there. Don't batch these up — capture them as they happen, while the reasoning is still in the conversation. Use the format in [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md).

`CONTEXT.md` should be totally devoid of implementation details. Do not treat it as a spec, a scratch pad, or a repository for implementation decisions. It is a glossary and nothing else.

### Offer ADRs sparingly

Only offer to create an ADR when all three are true:

1. **Hard to reverse** — the cost of changing your mind later is meaningful
2. **Surprising without context** — a future reader will wonder "why did they do it this way?"
3. **The result of a real trade-off** — there were genuine alternatives and you picked one for specific reasons

If any of the three is missing, skip the ADR. Use the format in [ADR-FORMAT.md](./ADR-FORMAT.md).

## Committing

`CONTEXT.md` and ADRs are committed artifacts, same as spec and plan docs. Commit them as part of the work that produced them — not in a separate "docs" commit at the end, where they'll be forgotten.

## Rationalization Table

| Excuse | Reality |
|--------|---------|
| "I'll add the glossary entries at the end" | You won't — the reasoning will be gone. Write it when the term resolves. |
| "Let me scan the codebase and bootstrap CONTEXT.md" | That's inference, not agreement. Only resolved ambiguities go in. |
| "The user said 'account', I know what they mean" | You inferred. If the glossary distinguishes Customer from User, ask. |
| "This is fast lane, I'll just name it and move on" | Minting vocabulary is not fast-lane work. Ask to reclassify. |
| "It's easier to put the implementation note in CONTEXT.md too" | It's a glossary. Implementation notes go in the spec or an ADR. |
| "This decision seems worth an ADR" | Run all three tests. Two out of three is a no. |
| "There's no CONTEXT.md, so there's no domain model to follow" | Then create one at the first resolved term. Absence isn't permission to be sloppy. |
| "I'll use their word this once and normalize later" | The word ships in identifiers. There is no later. |

## Red Flags — STOP

- About to write `CONTEXT.md` without having invoked this skill
- About to add a term nobody contested
- About to add a general programming concept (timeout, retry, DTO) to the glossary
- About to use a word the glossary lists under `_Avoid_`
- About to define a new domain term while in the fast lane
- About to write an ADR that fails any of the three tests
- Glossary and code disagree and you're proceeding anyway
