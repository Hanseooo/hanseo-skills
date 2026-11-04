# project-init

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Write or audit a repository's `AGENTS.md`: the exact commands, how to run the
app, the testing contract, and the unwritten rules an agent would otherwise break.

## Where this sits

Run it on any repo with code. It owns the "how do I work here" sections of
`AGENTS.md` and leaves the rest to their owners:

- the brief, roadmap and doc routing → `/groundwork`, which schedules
  project-init as its repo contract session
- architecture constraints and ADRs → `/charter`'s delimited block

It needs no other skill.

## The problem

Agent instruction files go wrong in predictable ways. They list commands
nobody ran, so an agent types `pnpm build` to typecheck and waits through a
full build. They cache the stack list from `package.json`, which goes stale on
the next upgrade. They end in a build log of what was done last month. And they
carry a review date that says nothing about whether anything was reviewed.

A generator that only reads the repo also misses what matters most. It can't see
what agents keep getting wrong or what's off limits for now. [One
study](https://arxiv.org/abs/2602.11988) found developer-written files beat
LLM-generated ones on task success, and that repository overviews didn't help.

## How project-init solves it

**Evidence or a person.** Every line traces to a file or to your own words. A
command it can't trace goes in the final report, not into the file with a
question mark beside it. The package manager comes from the lockfile.

**A short interview.** After reading the repo, it asks only what would change a
line in the file: what agents get wrong, what's out of scope, and any choice the
evidence can't settle. Questions are skippable, and with no way to ask, they go
in the report.

**Every line earns its load.** The file loads every session, so each line must
stop some mistake. Material only some tasks need sits behind a `Read when`
pointer. General behavior rules stay in your global config. For a team repo, it
adds a block of them when you say so or confirm it.

**An honest date.** `Last reviewed` appears only after lint, typecheck, unit
tests and build have run with your approval. Failures become Known Gaps.

**It works with what's there.** An existing file gets an audit and a diff, and
you choose patch, replace or stop. `/charter`'s block and other tools' sections
survive either way. It also warns when a `CLAUDE.md` or `CLAUDE.local.md` is
stopping `AGENTS.md` from loading in Claude Code.

## When not to use it

- **No code yet.** There is nothing to read commands from. Run `/groundwork`,
  which schedules this for later.
- **You want the project's direction, roadmap or doc map.** Run `/groundwork`.
- **You want architecture decisions recorded.** Run `/charter`.
- **A one-file script.** A README line on how to run it is enough.

## Install

Via the [skills.sh](https://www.skills.sh) CLI (works for Claude Code and other
supported agents):

```
npx skills add Hanseooo/hanseo-skills
```

Select `project-init` when choosing skills, or install the whole collection.

Or clone the collection and reference this skill's folder directly:

```
git clone https://github.com/Hanseooo/hanseo-skills
```

- **Claude Code:** clone into `~/.claude/skills/project-init` (use
  `skills/project-init/` as the skill root).
- **Codex, opencode, Antigravity CLI, or others:** clone anywhere and load
  `skills/project-init/SKILL.md` per that tool's own custom-instructions/skill
  mechanism, or paste `SKILL.md`'s contents into the session.

Update later with `git pull` (or re-run `npx skills add`).

## Use

```
/project-init
```

```
/project-init this repo is for our team of five
```

It reads the manifests, lockfiles, scripts, CI and docs, interviews you, then
writes the file. The final report lists each command with its source, the lines
it cut, whether the checks ran, and any repo problems it found.

## FAQ

### How is this different from Claude Code's `/init`?

`/init` writes a `CLAUDE.md` summarising the codebase, stack overview
included. project-init writes `AGENTS.md` for every tool, leaves out what one
lookup answers, asks about what the repo can't show, and refuses commands it
cannot trace. Want a quick tour of an unfamiliar codebase? `/init` is the better
choice.

### Do I still need a `CLAUDE.md`?

Not on current Claude Code, which reads `AGENTS.md` on its own from v2.1.277 when
no `CLAUDE.md` or `CLAUDE.local.md` is present. If one is present, add an
`@AGENTS.md` line to it.

## What it deliberately is not

- A codebase summary or onboarding guide.
- A place for global behavior rules, outside a team repo.
- A changelog: history stays in git, status in the roadmap.

## License

[MIT](LICENSE)
