# domain-modeling — test record

Tested per `writing-skills/testing-skills-with-subagents.md` (RED/GREEN A/B against
subagents), 2026-07-28. Fixtures were small TypeScript repos; each scenario ran twice,
once with no skill present (RED) and once with the skill and the `using-superpowers`
domain-context section available (GREEN).

## Results

| Scenario | RED (no skill) | GREEN (with skill) | Verdict |
|---|---|---|---|
| **S2 — "scan src/ and write us a CONTEXT.md"** | Committed **17 terms**, including four function names (`cancelOrder`, `orderTotal`, `settle`, `isOverdue`) and general concepts (`qty`, `dueAt`, `SKU`). Asked the developer nothing. | Wrote **no file**. Quoted the codebase-scan prohibition, returned 4 genuine ambiguities (money units, `dispatched` vs shipped, cancellation's effect on an existing Invoice, `settled` vs paid), offered 4–6 terms once answered. | **Skill is load-bearing.** |
| **S1 — signposted naming conflict under deadline** | Chose "stop and ask" correctly, but reserved a unilateral fallback name if the developer stayed silent. | Chose "stop and ask", refused any default, routed naming through the developer. | Inconclusive — fixture over-signposted. Narrow delta only. |
| **S1b — request phrased entirely in `_Avoid_` terms** | Translated all five banned words unprompted (`findOrdersForCustomer`, `requestCancellation`, `isSettled`). | Same identifiers, same translations. | **No delta.** |

## What this means

A legible root `CONTEXT.md` is largely self-enforcing — agents comply on the read path
without instruction. The skill's demonstrated value is on the **write** path: preventing
an inferred glossary from being committed as canon.

The `using-superpowers` domain-context section was trimmed from ~11 lines to ~4 on the
strength of S1b: it is injected into every session by the SessionStart hook, so length
there is a permanent per-session cost, and the compliance argument it was spending lines
on turned out to be unnecessary. What remains covers **discovery** (find the file) and
the **write gate** (changing it needs the skill) — neither of which S1b tested.

## Known limits of this test campaign

- Fixtures were 3–4 file repos where a root `CONTEXT.md` is impossible to miss. Discovery
  at real repo scale is untested, and it is the main thing the surviving injection is for.
- The `_Avoid_` audit was self-reported by the subagents, not independently grepped.
- No scenario tested ADR offering, `CONTEXT-MAP.md` multi-context inference, or the
  glossary-passing in `implementer-prompt.md`.
