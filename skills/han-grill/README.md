# han-grill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Stress-test a plan or idea through questions that explain the real options,
their benefits and costs, and the evidence behind a recommendation.

## Where this sits

Use during discovery, before `/han-to-spec` or another design-writing workflow.
It works independently and stops at a conversational agreement.

Adapted from Matt Pocock's
[`grill-me`](https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md)
and [`grilling`](https://github.com/mattpocock/skills/blob/main/skills/productivity/grilling/SKILL.md),
with approach-comparison ideas from Superpowers
[`brainstorming`](https://github.com/obra/superpowers/blob/v6.4.2/skills/brainstorming/SKILL.md).
References inspected 2026-10-20: Matt's `main` and installed Superpowers 6.4.2.
The interview is included here, so no upstream wrapper or skill is required.

## The problem

A question can ask you to choose without explaining what you give up. A
recommendation can sound decisive while relying on a goal nobody established.
In baseline trials, agents asked useful questions but varied in whether they
explained each cost or recommended an option.

## How han-grill solves it

It keeps the decision tree and independent-question rounds. Choice questions
have explicit benefit/cost slots and a recommendation grounded in known
priorities or facts. Unknown intent gets an open question first.

Answers reshape the next round. The recap preserves the choices, reasons and
remaining uncertainty for a later handoff.

## When not to use it

- **You need one factual answer:** ask directly or inspect the source.
- **Decisions are settled:** use `/han-to-spec` or your existing brief.
- **You want the upstream wrapper and interview unchanged:** use `/grill-me`.

## Install

Install through the [skills.sh CLI](https://github.com/vercel-labs/skills):

```sh
npx skills add Hanseooo/hanseo-skills --skill han-grill
```

Or browse the collection:

```sh
npx skills add Hanseooo/hanseo-skills
```

Standalone fallback:

```sh
git clone https://github.com/Hanseooo/hanseo-skills
```

- **Claude Code:** use `skills/han-grill/` as the skill root in your skills directory.
- **Codex and other agents:** load `skills/han-grill/SKILL.md` through the
  tool's skill mechanism. The bundled Codex policy keeps invocation explicit.

Update the clone with `git pull`, or re-run `npx skills add`.

## Use

```text
/han-grill the appearance inspector workflow
/han-grill this saved-filters idea; one question at a time
```

In Codex, invoke `$han-grill`. Expect a numbered round of independent choices,
then follow-ups based on your answers. You can modify an option, propose another,
delegate a decision or pause with remaining questions visible.

## FAQ

### Does this use strengths of brainstorming too?

Yes: approach comparison, explicit tradeoffs, contextual recommendations and
shared understanding. It retains grilling's dependency-aware questioning.
It does not import the full spec/plan/implementation sequence.

### Does every question need multiple choices?

No. An unknown audience or problem needs your description. Recommendations
depend on evidence, and the interview can explain what would help decide.

## What it deliberately is not

A spec generator, ticket publisher, implementation runner or automatic decision maker.

## License

[MIT](LICENSE). Includes upstream notices for Matt Pocock and Jesse Vincent.
