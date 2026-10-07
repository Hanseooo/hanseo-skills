# product-discovery

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

For deciding what independent software product to make, or whether to keep
going with one. It ends in a call such as ship tiny, validate, narrow, build or
kill, with the amount of proof sized to what being wrong would
cost.

## Where this sits

Upstream of everything else in this collection. Once the call is `build`, the
product is decided and design takes over: brainstorming for a single session,
`/docket` when the feature needs several, `/charter` for a new project's
decision layer.

It is model-invoked: the agent reaches for it on questions like "should I build
this?", "what could this new API enable?" or "the launch got attention but no
revenue". It stays out of implementation requests where the product is already
decided.

Tuned for self-serve consumer and prosumer products a stranger can find,
understand and buy without talking to the founder. It still works outside that
profile, with less to say about sales-led B2B.

## The problem

Ask an agent for product help and it is already decent at scepticism. In
baseline testing it correctly refused to pour marketing into a leaky funnel,
pushed back on three months of polish before any buyer was known, and
questioned a plan copied from two famous launches. Those cases needed little
help.

It failed in the generative and the rigorous parts:

- **Every idea was a painkiller.** Asked for ideas from a blank slate, it
  offered trade invoicing, CSV-to-report tools and client portals, then
  recommended one with "this kind of product looks boring and makes money".
  Nothing sold delight, identity or taste, and it picked a winner before
  knowing what the builder wanted.
- **New technology became wrappers.** Asked what a fast, cheap AI-judgment API
  could enable, it listed routing, moderation, record scoring and LLM
  guardrails — features inside someone else's product. It never asked whether
  the outcome mattered without the API, or whether plain code would do.
- **Competition was checked at category level.** For a "CleanShot for Windows"
  idea it planned to search the incumbents' names and Reddit, and counted
  "local and one-time purchase" as a strength. That search misses small indie
  products, old or recent, the competitors most likely to share the exact wedge.
  Its validation measured usage, not payment.
- **A timing launch had no launch.** For a ship-tonight opportunity it
  correctly said ship, then planned no duplicate check, no demo or channel, no
  plan for momentum and no stop rule — the parts that decide whether a
  timing play works at all.

## How product-discovery solves it

**Routing by where the user starts.** No idea, a concrete idea, a technology, a
closing window, a live product and a borrowed success story each start at a
different step, so a one-night experiment never gets a six-month product's
ceremony and a crowded idea never skips the competition check.

**Required slots where the baseline left things out.** Explore must offer
families that differ by the value they sell, with at least two that are not
painkillers. A technology start must write the technology-off, substitution and
role lines before any idea. A timing call must have all five parts of a timing
plan. The exact-competition search names where to look. Each one is a slot the
agent fills, not advice it can weigh away.

**Memory counts as assumption.** Competitors, prices and platform features
recalled without searching are labelled ASSUMPTION, and a wedge cannot be
called open until the search runs. The failure seen in testing was not
ignorance but confident recall of a market that may have moved.

**A decision, not a report, sized to the bet.** A call on weeks or more closes
with a six-line decision record: the call, confidence, strongest evidence, the
risk most able to kill it, the next action with a cap, and what would change
it. A weekend bet gets the same substance in a few short paragraphs, because
the baseline answered small questions well and the full framework only made
them longer.

## When not to use it

**The product is decided and you want it built.** Code, architecture,
debugging, UI — use the build and design skills directly.

**A single fact you could search for.** "What does Xnapper cost?" needs a
search, not a discovery session.

**Sales-led enterprise or venture decisions.** The patterns are drawn from
self-serve independent products. It will reason about a procurement-heavy B2B
product, but its references have little to offer there.

## Install

Via the [skills.sh](https://www.skills.sh) CLI (works for Claude Code and other
supported agents):

```
npx skills add Hanseooo/hanseo-skills
```

Or clone the collection and reference this skill's folder directly:

```
git clone https://github.com/Hanseooo/hanseo-skills
```

- **Claude Code:** clone into `~/.claude/skills/product-discovery` (use
  `skills/product-discovery/` as the skill root).
- **Codex, opencode, Antigravity CLI, or others:** clone anywhere and load
  `skills/product-discovery/SKILL.md` per that tool's own
  custom-instructions/skill mechanism, or paste `SKILL.md`'s contents into the
  session.

Update later with `git pull` (or re-run `npx skills add`).

## Use

Ask the product question in plain words; the agent loads the skill itself.

```
Should I build a screenshot beautifier for Windows? There's CleanShot on Mac.
A new on-device model API just launched. What could I make with it?
Our launch got 2M views but trial-to-paid fell from 4% to 1.5%. More features?
```

Or invoke it directly with `/product-discovery`.

A blank-slate or technology start comes back as a few directions that sell
different things, plus at most three questions. A concrete idea comes back as a
draft thesis with the reason-to-win left for you to fill, the searches that
would settle it, and a capped experiment. A "should I" on anything bigger than
a weekend ends in a decision record. Ask it to rank directions once you have
reacted to them and it will. Expect it to ask what you want from the product —
a first paid product, a durable business and a weekend experiment get different
answers.

## What it deliberately is not

Not an idea generator: directions are offered for you to react to, and not
ranked before you have. Not a market-research report. Not a startup scorecard —
nothing is scored. Not a launch-marketing playbook; distribution is reasoned
from the product, never prescribed by channel.

## License

[MIT](LICENSE)
