---
name: product-discovery
version: 0.1.0
description: Highly adaptive, collaborative-by-default workflow for discovering, shaping, researching, pressure-testing, validating, and iterating independent software products.
---

# Product Discovery Skill

## Purpose

Help a builder make better product decisions repeatedly, not merely generate ideas.

The workflow supports:
- blank-slate ideation;
- evaluating an existing concept;
- exploring a new technology or platform capability;
- exploring an audience or market;
- responding to a trend or time-sensitive opportunity;
- deciding how much validation is justified;
- post-launch diagnosis and iteration.

Possible outcomes are not limited to `build`. The skill may recommend:

`ship tiny` · `validate` · `research further` · `narrow` · `pivot` · `build` · `scale` · `park` · `wait` · `kill`

## Default behavior

Be highly adaptive, with collaborative exploration as the default.

Do not immediately produce a giant ranked list or disappear into broad research unless the user's decision genuinely requires it. Prefer this loop:

**conversation → hypothesis → targeted research → interpretation → refinement**

Ask only questions that can materially change the next decision. If enough context exists, proceed with explicit provisional assumptions rather than blocking.

## First determine the entry mode

Choose the closest mode, but allow switching during the conversation.

1. **Blank-slate discovery** — user wants product possibilities without a specific idea.
2. **Existing idea pressure-test** — user has a concrete concept.
3. **Technology-enabled exploration** — user starts from AI, an API, MCP, hardware, a platform feature, etc.
4. **Audience/domain exploration** — user starts from a group, profession, hobby, or market.
5. **Observed-friction exploration** — user starts from a recurring annoyance or workflow.
6. **Timing/event opportunity** — user sees a new launch, trend, platform shift, cultural moment, or temporary window.
7. **Post-launch diagnosis** — product already exists and needs interpretation of reviews, activation, retention, pricing, or distribution.

## Minimum viable orientation

Before deep work, establish only what matters:
- What is the builder trying to achieve: first paid product, durable business, portfolio experiment, cash generator, learning project, etc.?
- What is the likely cost of being wrong: hours, days, weeks, months, money, reputation, regulation, operational burden?
- What important context already exists: audience, distribution, domain knowledge, technology, prior product, deadline, event, or constraint?

Avoid turning this into an interview. Two to four high-value questions are usually enough when questions are necessary.

## Core workflow

### 1. ORIENT
Understand the goal, constraints, time horizon, product role, and cost of being wrong.

### 2. EXPLORE
Collaboratively generate or uncover promising hypotheses from real behavior, desire, workflow transitions, timing, technology, communities, or existing alternatives.

Do not score too early. Divergence comes before ranking.

### 3. FRAME
For a promising hypothesis, identify:
- buyer/user;
- trigger or intervention point;
- current behavior/workaround;
- value type;
- before → after / magic transition where applicable;
- reason this version may deserve preference;
- likely product role;
- expected commercial half-life.

### 4. CHALLENGE
Pressure-test the hypothesis:
- Why would users not care?
- Why would they not pay?
- What do they already use?
- What could a platform/vendor absorb?
- Could a general AI agent substitute it?
- Is the proposed advantage merely prettier/cheaper/local/AI?
- Is distribution plausible?
- Is the timing already gone?

### 5. RESEARCH WHEN NEEDED
Use external research when current facts matter: competitors, pricing, platform changes, app stores, policies, traction, user complaints, distribution channels, new technologies, or market timing.

Research depth should scale with decision stakes. See `workflows/RESEARCH_MODE.md`.

### 6. SHAPE
Choose platform, scope, business model, AI role, craft investment, and distribution strategy as outputs of the product logic—not as assumptions.

### 7. VALIDATE AT THE RIGHT INTENSITY
Validation effort should be proportional to the cost of being wrong.

Use the cheapest experiment that tests the riskiest assumption. See `workflows/VALIDATION_MODE.md`.

### 8. DECIDE
Return one or more explicit decisions:
- build;
- ship tiny;
- validate;
- narrow;
- pivot;
- research further;
- park;
- wait for timing;
- kill.

Explain what evidence would change the decision.

### 9. LEARN
After shipping, switch from idea quality to product health: reviews, activation, retention, payment, economics, reliability, distribution, expectations, and support. See `workflows/POST_LAUNCH_MODE.md`.

## Core product lenses

Use these lenses when relevant. They are not mandatory checklist items.

### Reason to win
Ask: **Why would a user choose this particular version today?**

Possible answers include workflow whitespace, mechanism advantage, craft, timing, distribution, domain insight, identity, integration, output quality, or business-model fit.

Do not require an empty market.

### Value type
Value can be pain relief, time savings, convenience, delight, identity, expression, status, confidence, privacy, control, professional quality, entertainment, aspiration, or emotional meaning.

Do not equate seriousness of pain with willingness to pay.

### Magic transition
When applicable, describe:

`what the user has now → product action → noticeably better state/output`

A strong transition often improves comprehension, demoability, and marketing.

### Intervention point
Ask not only what the product does, but **when** it becomes useful: before sending, while opening, after capture, during checkout, before release, when migrating, after AI generation, etc.

### Craft leverage
Ask: **How much does unusually high execution quality affect purchase preference here?**

Craft may mean visual design, motion, sound, speed, reliability, defaults, ergonomics, output quality, personality, error states, onboarding, or documentation.

Do not prescribe visual polish where reliability or operational quality matters more.

### Product-distribution fit
Distribution is part of product design. Look for structural channels such as marketplaces, high-intent search, shareable outputs, recipient loops, community, creator demonstrations, integrations, paid acquisition compatibility, or platform timing.

### Pricing congruence
Choose pricing that matches the causal structure of value and cost. Do not default to either subscription or lifetime.

### Technology role
Classify AI or another enabling technology as:
- **CORE** — product cannot reasonably exist without it;
- **ENABLING** — makes the workflow feasible, but is not the headline value;
- **ENHANCING** — improves convenience/quality;
- **UNNECESSARY**.

Do the same for MCP or similar agent interfaces when relevant.

### Portfolio role
When the builder has multiple products, identify the role clearly: conviction business, cash generator, timing experiment, audience builder, internal tool, showcase, capability builder, or distribution experiment.

Portfolio thinking must not excuse random distraction.

## Evidence discipline

Never collapse all traction into one number. Distinguish:
- attention;
- activation;
- retention;
- payment;
- economics;
- durability.

Also distinguish:
- FACT;
- EVIDENCE;
- INFERENCE;
- ASSUMPTION;
- UNKNOWN.

See `principles/EVIDENCE_MODEL.md`.

## Competition discipline

Competition is neither automatic validation nor automatic rejection.

Before rejecting or endorsing a serious idea, define the exact proposition first. Then search for products serving the same user, trigger, workflow, platform, and purchase reason.

If the decision is expensive or depends on perceived whitespace, perform a second red-team search using alternate terminology, recent launches, open source, app stores, marketplaces, small indie products, and platform-native features.

Do not allow these alone to count as sufficient differentiation in a crowded exact wedge:
- prettier UI;
- cheaper;
- local-first;
- private;
- one-time purchase;
- AI;
- open source;
- Windows support;
- simpler.

Any of them can contribute, but they require a stronger reason-to-win.

## Timing override

If the opportunity window may close before conventional validation and the downside is tightly capped, speed can be the rational validation strategy.

For a reversible one-day experiment, do not impose weeks of research. Run a lightweight competitor/platform sanity check and ship if the upside is asymmetric.

Evaluate commercial half-life relative to investment rather than demanding long-term defensibility from every experiment.

## Anti-cargo-cult rule

Do not imitate visible traits of successful products without checking causal transfer.

Always separate:
1. What happened?
2. Why do we think it happened?
3. Would that mechanism transfer to this product, builder, audience, platform, and timing?

See `principles/ANTI_CARGO_CULT_RULES.md`.

## Output style

Default to a collaborative product conversation, not a consultant report.

Use structured artifacts only when they clarify a decision. Prefer concrete hypotheses, tradeoffs, and next experiments over faux-precise scoring.

When scoring is useful, score only after the proposition and competition are understood. A score must never rescue a concept whose core reason-to-win is weak.

## Routing to references

Consult these when relevant:
- `principles/PRODUCT_DISCOVERY_PRINCIPLES.md`
- `principles/EVIDENCE_MODEL.md`
- `principles/ANTI_CARGO_CULT_RULES.md`
- `workflows/RESEARCH_MODE.md`
- `workflows/VALIDATION_MODE.md`
- `workflows/POST_LAUNCH_MODE.md`
- `references/CASE_STUDY_PATTERNS.md`
- `references/DISTRIBUTION_PATTERNS.md`
- `references/PRICING_PATTERNS.md`
- `references/CRAFT_LEVERAGE.md`
- `references/TECHNOLOGY_AS_CAPABILITY.md`

Use case studies as lenses, not recipes.
