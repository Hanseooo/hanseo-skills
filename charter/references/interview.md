# Interview question bank

One question at a time. Ask only what the doc/code inventory did not answer —
never re-ask what a doc answers; confirm the inference instead ("Your PRD implies
X — locking that in, correct?"). Batch closely related questions via multi-select
where safe.

**Budget by dial:** prototype ≤3 questions, product ≤6, platform ≤10. When the
budget runs out, propose defaults for the remaining gaps instead of asking.

## The dial (always first, one question)
"Is this a throwaway prototype, a real product, or a long-lived platform?"
- prototype → half-page contract + 2–3 ADRs, no architecture.md
- product → all three artifacts, lean
- platform → full treatment

## Change scenarios (core bank — pick per gap)
Volatility determines where abstraction belongs, so target it directly:
1. What do you expect to swap out? (models, providers, databases, payment, hosting)
2. What will grow? (users, data volume, team size, feature surface)
3. Who maintains this in a year — you, a team, or mostly agents?
4. What must a future agent never do? (touch billing, call vendors directly,
   commit secrets, change the schema without review…)
5. What is fixed by outside forces? (compliance, platform, budget, deadline)
6. Where did your last project like this go wrong?

## Greenfield variants (Route A)
- "What's the first thing you'll build, and what does it talk to?"
- "Which technical choice are you least sure about? That's where a seam belongs."

## Early-code variants (Route B)
Ask against the code inventory, never in the abstract:
- "I see <pattern> — deliberate decision, or accident of speed?"
- "Which parts of the current code are you willing to rewrite, and which are
  load-bearing?"

## Rules
- One pushback on any default = record the user's choice and proceed. Never argue
  past one pushback.
- After the doc inventory, always close with: "Any docs I missed — or external
  ones (Notion, wiki)?" before treating a gap as real.
