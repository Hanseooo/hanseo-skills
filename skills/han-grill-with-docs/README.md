# han-grill-with-docs

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Run a `/han-grill` interview that writes settled terms and hard-to-reverse
decisions into your project's glossary and decision record as they settle.

## Where this sits

Use during discovery, in place of `/han-grill`, when the conversation is
likely to fix vocabulary or make architectural choices worth recording. It
hands off to `/han-to-spec` or any other spec workflow.

A thin wrapper, like Matt Pocock's
[`grill-with-docs`](https://github.com/mattpocock/skills): it loads
`/han-grill` and Matt's `/domain-modeling` and adds two rules on top. It
requires both skills and stops with their install commands if either is
missing.

## The problem

Matt's `domain-modeling` writes to `GLOSSARY.md` and `docs/adr/`. Many
projects already keep vocabulary and decisions elsewhere: this repo uses
`CONTEXT.md`, and another project keeps numbered decisions in a single table.
A second glossary or ADR folder beside the existing one splits the record, and
later sessions read only one of them.

## How han-grill-with-docs solves it

It finds the existing glossary and decision record before the first question
and writes there in that file's own format and numbering. Matt's layout is the
fallback for projects with neither.

Doc edits are the only writes the interview makes. The closing recap lists each
one, so the record and the agreement can be reviewed together.

## Differences from Matt's

- **Existing docs first.** It finds the project's own glossary and decision record before the first question and writes there, in that file's format and numbering. Matt's `grill-with-docs` writes `GLOSSARY.md` and `docs/adr/`. That layout is only the fallback here.
- **Doc edits are the only writes**, and the closing recap lists each one for review.
- **Interview comes from `/han-grill`**, so it has rounds with costed options, not Matt's one-question flow.

## When not to use it

- **No terms or lasting decisions are at stake:** use `/han-grill`.
- **You only need to read the glossary:** any skill can do that; this one
  changes it.
- **You want Matt's file layout regardless of existing docs:** use
  `/grill-with-docs`.

## Install

Install through the [skills.sh CLI](https://github.com/vercel-labs/skills),
together with its two dependencies:

```sh
npx skills add Hanseooo/hanseo-skills --skill han-grill-with-docs
npx skills add Hanseooo/hanseo-skills --skill han-grill
npx skills add mattpocock/skills --skill domain-modeling
```

Or browse the collection:

```sh
npx skills add Hanseooo/hanseo-skills
```

Standalone fallback:

```sh
git clone https://github.com/Hanseooo/hanseo-skills
```

- **Claude Code:** use `skills/han-grill-with-docs/` as the skill root in your
  skills directory.
- **Codex and other agents:** load `skills/han-grill-with-docs/SKILL.md`
  through the tool's skill mechanism. The bundled Codex policy keeps invocation
  explicit.

Update the clone with `git pull`, or re-run `npx skills add`.

## Use

```text
/han-grill-with-docs the context-menu ownership model
```

In Codex, invoke `$han-grill-with-docs`. You get `/han-grill`'s numbered rounds.
When a term or decision settles, the agent edits the glossary or decision
record in that turn and says which entry changed.

## FAQ

### How is this different from `/grill-with-docs`?

The interview is `/han-grill`'s, with explicit benefit and cost per option. Doc
writes go to the project's existing files instead of always to `GLOSSARY.md`
and `docs/adr/`. Use `/grill-with-docs` if you want Matt's interview and layout
unchanged.

## What it deliberately is not

A spec writer, a doc generator for existing code, or a replacement for
`/domain-modeling` outside an interview.

## License

[MIT](LICENSE). Includes upstream notices for Matt Pocock and Jesse Vincent.
