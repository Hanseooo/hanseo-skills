---
name: product-discovery
description: Use when deciding what independent software product to make, or whether to keep going with one — "should I build this?", "I want a paid product but don't know what", "is this worth it with competitors around?", "what could this new API / model / platform feature enable?", "this just launched, should I ship something tonight?", "the launch got attention but no revenue", or a pricing, pivot or kill call on an app. Not for implementation requests where the product is already decided — code, architecture, debugging, UI work.
---

# product-discovery

Product thinking for independent software: what to make, whether to keep making
it, and how much proof each call deserves. The work ends in a **decision**, not
an idea list or a report:

`ship tiny` · `validate` · `research further` · `narrow` · `pivot` · `build` · `scale` · `wait` · `park` · `kill`

A kill that saves months is a good outcome. Work as a collaborator: think out
loud with the user, question premises, let the idea move, and go back a step
whenever new evidence changes the thesis.

**Size the reply to the bet.** When being wrong costs days or less and is easy
to undo, reply in a few short paragraphs: the call, the risk most able to kill
it, the cap, and what would mean continue or stop. The thesis template, the
research steps and the full decision record are for bets of weeks or more, or
when the user asks for them. A timing plan keeps its five parts, a line each.

## Route by where the user starts

| The user starts from | Begin at | Load |
|---|---|---|
| No idea yet | Orient → Explore | — |
| A concrete idea | Orient → Frame | — |
| A technology, API, model or platform capability | Explore → Technology | — |
| An audience, community or recurring annoyance | Orient → Explore | — |
| A launch, trend or event with a closing window | Timing | — |
| A live product with numbers or reviews | Post-launch | — |
| A cited success story as the plan ("X did Y, so I'll…") | Challenge → Borrowed lessons | `references/case-patterns.md` |
| A pricing question | Shape → Pricing | `references/pricing.md` |

Switch rows when the conversation does.

## Orient

Know three things before deep work: what the builder wants from this (first paid
product, durable business, cash, experiment, learning, showcase), what being
wrong costs (hours to months; money, reputation, regulation, support load), and
what they already hold (audience, domain knowledge, tech, deadline). Ask at most
three questions, and put concrete material in the same reply so the user has
something to react to. A gap that does not change the next step becomes a
stated provisional assumption.

## Explore

Offer 3–5 **hypothesis families** that differ by value type, not just topic.
Value types: pain relief, time, money, delight, identity or expression,
confidence or control, status, emotional meaning, professional output quality.
**At least two families sell something other than pain relief, time or money.**
Products people simply want — a satisfying experience, a gift, an object of
taste — can earn real willingness to pay without relieving any pain. Never drop
a direction because its value is desire rather than relief.

Places families come from: one recurring irritation in a category people already
use; the step just before or after an established product does its job, where no
participant owns the hand-off; a community that cares intensely about an
experience; a workflow the builder knows that general products ignore; a wanted
before → after transformation; a newly feasible capability.

Do not rank directions before knowing what the user wants from this. Ask which
family they recognise or feel pull toward, then Frame that one. Rank when they
ask, or once evidence makes a comparison useful.

### Technology

When the start is a capability, write these three lines before any idea:

- **Technology-off:** the user's outcome stated without naming the technology.
  An outcome that stops mattering is a wrapper; drop it.
- **Substitution:** could deterministic code, a local model, a general LLM or a
  human confirmation step deliver it? The new capability must win on something
  the end user feels — latency, cost, privacy, reliability.
- **Role:** CORE (cannot exist without it), ENABLING, ENHANCING or UNNECESSARY.
  MCP or agent interfaces: CORE, USEFUL EXTENSION or UNNECESSARY.

Families are products an end user buys for an outcome; classification, ranking
or filtering can be the mechanism inside one. Generic routing, moderation,
scoring and guardrails are features inside someone else's product: offer them
as products only when the builder sells to developers.

## Frame

Write the thesis before any competitor search:

> For **[specific user]**, when **[trigger]**, move them from **[current state]**
> to **[better state]** by **[mechanism]**. They choose this version because
> **[reason-to-win]**.

**Reason-to-win** answers: why would someone choose this particular version
today? Workflow whitespace, a better mechanism, craft, output quality, timing,
distribution, domain insight, identity or community fit, integration,
business-model fit. An empty market is one possible answer, never a requirement.

Also name, where it applies: the **intervention point** (the moment it is most
valuable — before sending, on opening, before release); the visible before →
after; **craft leverage** (how far unusually good execution moves preference —
motion, sound and output for creator and delight tools; reliability and
correctness for plumbing); the product's **role** (conviction product, cash
generator, timing experiment, audience builder, showcase, internal tool); and its
expected **commercial half-life**. A builder juggling several products names
each one's role; a product without a clear one may be distraction rather than
leverage.

## Challenge

Attack the thesis before defending it. Why would they not care, or not pay? What
do they use now? Could the platform or a general AI agent absorb it? Is
acquisition the real bottleneck? Is personal taste standing in for demand?

**Additive, never sufficient alone in a wedge with exact competitors:** prettier
UI, cheaper, local-first, private, one-time purchase, AI, open source, another
platform, simpler. Each strengthens a reason-to-win; none is one.

**Borrowed lessons.** When a successful product is the model, separate what
happened, why it is believed to have happened, and whether that mechanism exists
here — same audience, distribution, timing, platform, founder assets. Transfer
mechanisms, never visible traits. `references/case-patterns.md` holds studied
mechanisms and counterexamples.

## Research

Research what changes the decision, at a depth set by the cost of being wrong:
a light sanity check for cheap reversible bets, focused research for a 1–4 week
build, a deep pass for months-long, regulated or support-heavy ones. Competitors,
prices and platform features recalled from memory count as ASSUMPTION until
searched.

**Exact-competition search** — required before calling any wedge open:

1. Thesis sentence written first.
2. Search the exact workflow in the user's words, the trigger's words and the
   outcome's words — not only the category or the incumbents' names.
3. Search where small products live — app stores, extension and plugin
   marketplaces, GitHub — counting any product still active, whatever its age.
   Add indie launch posts and Reddit threads from the last two years, where
   recent entrants show up before search ranks them.
4. Check the platform's native features.
5. For a weeks-or-more decision, red-team: assume the whitespace is false and
   search again with different vocabulary.

Report the closest exact competitors, what survives of the reason-to-win, and
what is still unknown. Whitespace gone → narrow, pivot or kill. Broad demand for
a category says nothing about whether this wedge is open.

## Shape

Platform, MVP boundary, business model, AI role, craft emphasis and distribution
follow from the thesis. Name where user #1 comes from and what repeats for user
#10; when neither has an answer, load `references/distribution.md`. When choosing
or changing a price model, load `references/pricing.md`.

## Validate

Name the risk most able to kill it — value, payment, distribution, technical,
platform, retention, trust — and pick the cheapest experiment that hits that
risk, which is often not the easiest thing to prototype. Effort scales with
build cost × irreversibility × operational risk × uncertainty: shippable in days
with negligible downside, shipping is the experiment; months-long, regulated or
support-heavy, evidence comes first.

Every experiment states three things: the signal to continue, the signal to stop
or narrow, and the time and money cap before re-deciding. When payment is the
risk, the signal involves money — a price on the page, a preorder, a paid beta.
When craft is the value under test, the experiment is a small genuinely polished
slice; a crude prototype removes the variable being tested.

Evidence, strongest first: payment from target users, behaviour in a real
workflow, repeat use, interviews about past behaviour, reviews and complaints,
paid adjacent products, search and discussion volume, signups, likes and views.

## Timing

A window that closes before ordinary validation, with capped downside, gets a
**timing plan** with five parts:

1. **Sanity check, under an hour:** has someone already shipped this; IP,
   trademark or platform-policy exposure; does the core capability exist on the
   target hardware or platform.
2. **Caps:** build hours and spend.
3. **Launch content and channel, decided before building:** the demo, where it is
   posted, who would amplify it. Inside a window, distribution is most of the
   product.
4. **Momentum plan** if it lands: the next 48 hours — follow-up post, price,
   waitlist for a fuller version.
5. **Stop rule:** the sign the window has closed.

Judge it by upside against investment over the expected half-life, not by
defensibility. Builders who already know a capability are the ones able to move
when the trigger arrives.

## Post-launch

Once live, diagnose behaviour instead of re-arguing the idea. Separate
attention → activation → retention → payment → economics → durability; a launch
can be strong on the first and weak on every other. Sort negative reviews by
cause — reliability, expectation gap, pricing or paywall, missing core behaviour,
onboarding, performance, trust — before adding features or spend. Split
conversion and reviews by cohort, source and country; one market can be failing
while another is fine. A viral cohort converting worse than an
earlier one points first to audience mismatch; compare onboarding, pricing,
reliability and expectations across the cohorts before blaming the product. A
product worth keeping
but not focusing on gets a role: maintained utility, audience asset, showcase,
open-source, or retired.

## Decide

When the conversation reaches a call on a bet of weeks or more, or the user asks
for one, give a **decision record**:

- **Decision:** one from the list at the top
- **Confidence**
- **Strongest evidence** for it
- **Dominant unresolved risk**
- **Next action**, with its cap
- **What would change the decision**

Label the claims the decision rests on: FACT (checked source or observed
behaviour), INFERENCE, ASSUMPTION, UNKNOWN.
