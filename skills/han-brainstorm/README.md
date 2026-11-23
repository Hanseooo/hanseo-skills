# han-brainstorm

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Turn a vague itch ("people leave early, maybe I should build something?") into
a brief you have confirmed: who has the problem, what should change, how you
will know it worked, and what is off limits.

## Where this sits

Before `/han-grill`. han-brainstorm settles what and why; `/han-grill` takes
the brief as its intake and settles how, starting with a comparison of whole
approaches. Small, obvious changes can be built straight from the brief. A
question about whether a product should exist at all belongs to
`/product-discovery`.

Distilled from Superpowers
[`brainstorming`](https://github.com/obra/superpowers) (get the user
describing, one question per message, playback in parts) and Every's
[`ce-brainstorm`](https://github.com/EveryInc/compound-engineering-plugin/tree/main/skills/ce-brainstorm)
(a checkable exit, the integration check, assumptions written down), in the
short style of Matt Pocock's skills. References inspected 2026-11-23:
Superpowers 7.0.0 and ce-brainstorm on `main`.

## The problem

In baseline trials with no skill, agents asked good questions and played back
well, but all three proposed a solution mid-conversation and closed by telling
the user to build it, skipping the choices still open. None noticed that two of
the user's own facts collided: asking for notification permission later meant
no reminders during the very days the success metric counted. None marked an
assumption.

## How han-brainstorm solves it

- **What and why only.** Solutions that come up become questions about the
  problem they would solve, so the how is decided in `/han-grill`, with costed
  options, instead of slipping in during discovery.
- **A checkable end.** Actor, outcome, success and boundaries are each stated
  or written down as an assumption, then one integration check combines two of
  your statements to find the consequence neither mentions.
- **A routed close.** One suggested next step: `/han-grill`, build, stop, or
  `/product-discovery`. You start it.

## When not to use it

- **You already know the goal and need choices made:** use `/han-grill`.
- **You want a written design doc and implementation plan:** use Superpowers
  `brainstorming`, which hands to `writing-plans`.
- **The task is clear and small:** just ask for it.

## Install

Via the [skills.sh](https://www.skills.sh) CLI:

```sh
npx skills add Hanseooo/hanseo-skills --skill han-brainstorm
```

Or browse the collection:

```sh
npx skills add Hanseooo/hanseo-skills
```

Standalone fallback:

```sh
git clone https://github.com/Hanseooo/hanseo-skills
```

- **Claude Code:** use `skills/han-brainstorm/` as the skill root in your
  skills directory.
- **Codex and other agents:** load `skills/han-brainstorm/SKILL.md` through the
  tool's skill mechanism. The bundled Codex policy keeps invocation explicit.

Update the clone with `git pull`, or re-run `npx skills add`. It suggests
`/han-grill` but runs without it.

## Use

```text
/han-brainstorm my app seems to lose people early on
```

In Codex, invoke `$han-brainstorm`. Expect one question per message, short
playbacks to correct, then a full brief to confirm and one suggested next step.

## FAQ

### How is this different from Superpowers `brainstorming`?

`brainstorming` carries an idea all the way to a written design and hands only
to `writing-plans`. han-brainstorm stops at a confirmed brief in the chat,
leaves every how-choice to `/han-grill`, and adds the integration check and
written assumptions. Use `brainstorming` when you want its design doc and plan.

## What it deliberately is not

A design session, an approach comparison, a spec writer or a product-viability
call.

## License

[MIT](LICENSE). Includes the upstream notice for Jesse Vincent.
