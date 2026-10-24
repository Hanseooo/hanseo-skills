# project-init

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Write or audit a repository's `AGENTS.md` from what the repo itself shows: the
exact commands, the package manager, the testing contract and the invariants an
agent would otherwise break.

## Where this sits

Run it on any repo with code. It owns the "how do I work here" sections of
`AGENTS.md` and leaves the rest to their owners:

- the brief, roadmap and doc routing → `/groundwork`, which schedules
  project-init as its M0 repo contract session
- architecture constraints and ADRs → `/charter`'s delimited block

It needs no other skill.

## The problem

Agent instruction files go wrong in predictable ways. They list commands
nobody ran, so an agent types `pnpm build` to typecheck and waits through a
full build. They cache the stack list from `package.json`, which goes stale on
the next upgrade. They end in a build log of what was done last month. And they
never say when to update themselves, so the next session trusts a file that
stopped being true weeks ago.

There is also a quieter failure on Claude Code. Tested without this version,
an agent wrote a good `AGENTS.md` and no `CLAUDE.md`, "since the user did not
ask". Claude Code reads `CLAUDE.md`, so the new file never loaded.

## How project-init solves it

**Evidence or nothing.** Every command matches a script, target or CI step. One
it cannot trace goes in the final report, not into the file with a question
mark beside it. The package manager comes from the lockfile.

**Every line earns its load.** The file loads every session, so each line must
stop some mistake. Material only some tasks need sits behind a `Read when`
pointer to the doc that holds it. General behavior rules stay in your global
config, except in repos worked by several agent tools.

**It loads, and it stays current.** A one-line `CLAUDE.md` imports `AGENTS.md`.
A "Keep docs current" table says which section to update when a script, a
toolchain pin or a known gap changes, and it shares that table with the other
skills that write `AGENTS.md`.

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
/project-init fill in the commands and testing contract
```

It reads the manifests, lockfiles, scripts, CI and docs, and checks for other
agent tools' instruction files and duplicate worktrees. An existing file is
never overwritten: you get a diff and choose patch, replace or stop. The final
report lists each command with its source, the lines it cut, and the gaps it
left unfilled rather than guess.

## FAQ

### How is this different from Claude Code's `/init`?

`/init` writes a `CLAUDE.md` summarising the codebase, stack overview
included. project-init writes `AGENTS.md` for every tool, leaves out what one
lookup answers, and refuses commands it cannot trace. Want a quick tour of an
unfamiliar codebase? `/init` is the better choice.

## What it deliberately is not

- A codebase summary or onboarding guide.
- A place for global behavior rules.
- A changelog: history stays in git, status in the roadmap.

## License

[MIT](LICENSE)
