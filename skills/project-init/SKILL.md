---
name: project-init
description: Write or audit a repository's AGENTS.md from repo evidence - commands, tooling, testing contract and invariants. Usage: /project-init [project context]
disable-model-invocation: true
---

# project-init

Write or audit a repository's `AGENTS.md` from evidence found in that
repository. Project instructions hold repository-specific guidance; global
behavior stays global.

Where this says "ask", use the harness's question tool; with none, state the
question and stop. Where it says "inspect", use read-only file tools (glob,
read, search). Some setups allow no shell, and a failed shell call can abort
the run partway through.

## Phase 0: Safety check

Locate the repository root. No version control → treat the working directory as
the root and say so in the final report. From the root down to the working
directory, check for agent instruction files: `AGENTS.override.md`, `AGENTS.md`,
`CLAUDE.md`, `GEMINI.md`, `.cursor/rules/`, `.cursorrules`,
`.github/copilot-instructions.md`, `.windsurfrules`.

- **None exist** → Phase 1.
- **Any exists** → read it and keep it. Run Phase 1, then report what it gets
  wrong or omits, and what it carries inline that only some tasks need. Propose
  a diff and ask whether to patch in place, replace, or stop. Stop means stop.

Sections other skills own survive every patch and every replace: a delimited
block (`<!-- charter:begin -->` … `<!-- charter:end -->`), the Documentation Map
rows pointing at docs that exist, and the rows of the Keep docs current table.
Add rows to them; the rest of their text stays as it is. A placeholder line
such as "Commands arrive with M0's repo contract session" is yours to replace.

## Phase 1: Reconnaissance

Gather evidence for every claim you will make:

- Root listing; manifests and lockfiles (`package.json`, `pyproject.toml`,
  `go.mod`, `Cargo.toml`, `Gemfile`, `pom.xml`, `build.gradle`, `composer.json`,
  `Makefile`, `requirements*.txt`). Unfamiliar ecosystem → find its manifest and
  lockfile from the root listing; this list is a start, not a limit.
- Monorepo config: `pnpm-workspace.yaml`, `turbo.json`, `nx.json`, `lerna.json`,
  `Cargo.toml` workspaces, `go.work`.
- Scripts: manifest scripts with their bodies, `Makefile` targets, CI workflow
  steps, `justfile`, `Taskfile.yml`.
- Docs: `README*`, `docs/**/*.md`, root `*.md`, and any architecture, design,
  spec, brief or roadmap doc.
- Test layout, fixtures, and nested agent instruction files.
- Environment pinning: virtualenv directories, `.tool-versions`, `.nvmrc`,
  container and devcontainer definitions.
- Duplicate trees: `.worktrees/`, `.git/worktrees`, vendored or bundled copies.
  A repo-wide search hits every copy, and a hit inside one is another branch's
  state. Find them now, before any search you rely on.

Executable configuration, CI and code beat prose. Read enough to verify, not
everything.

**Checkpoint:** an install, lint, test or verify command with two candidates the
evidence cannot decide between → ask one focused question before writing files.

## Phase 2: Write AGENTS.md

`AGENTS.md` goes at the repository root. In a monorepo the root carries what is
true everywhere and names the packages; a package gets a nested `AGENTS.md` only
when its commands, package manager or test contract differ, and that file states
only the difference. Two independent projects sharing a repo with no root
tooling is not a monorepo: one root `AGENTS.md`, and every command names the
directory it runs from.

Include only sections that change agent behavior and that evidence supports.
Delete any section you cannot fill. A stack summary one manifest answers stays
out.

```md
# <Repo> Agents Configuration

Last reviewed: <YYYY-MM-DD>

## Project Context
- Architecture: <one paragraph, only what the file layout does not already say>
- Critical paths: <auth, billing, data pipelines, infra>, require extra review.

## Documentation Map
- Always read: <1-3 highest-signal docs>
- Read when: <task that needs it> → <doc path>
- Fallback: no docs beyond README → README, CI config, and code are truth.
- Where a doc above answers the question, it outranks commit messages, roadmap
  history, and any skill that fires on its own. Design skills are reviewers,
  not requirements.

## Commands (Use Exactly)
- Install: <command>
- Lint: <command>
- Build/Typecheck: <command>
- Unit tests: <command>
- Integration/E2E: <command>
- Pre-merge verify: <ordered sequence>
- Single test: <runner> <flags> path/to/test -t "name"

## Tooling Lock
- Canonical package manager: <name>. Never use: <the others>.
- Toolchain-pinned invocation only (`.venv/bin/python -m ...`, `uv run`,
  `poetry run`, the version in `.nvmrc`). Never install globally. Confirm cwd and
  active toolchain before any install or test.

## Testing Contract
- What "passing" means here: <...>
- CI: <workflow> on <trigger>. <Gates merge | reports only>
- Intentional skips and known flaky: <...>
- Mock boundary: <the third-party clients, clock and network seams tests may
  stub>. Everything inside it runs for real.
- Fixtures: <where captured real payloads live>
- Critical flows: <user-visible flows a change must be verified through end to end>
- Not automatable: <hardware, devices, third-party dashboards> → manual check,
  reported as unverified

## Project Invariants
- <Fragile rules: coordinate systems, ID ordering, required boundary conversions>

## Known Gaps
- <Open problems an agent would otherwise rediscover. Ask before fixing one, and
  delete its line in the same change.>

## Definition of Done
- <project-specific merge or PR requirements>

## Keep docs current
| When | Update |
|---|---|
| A script, CI step or command changes | Commands, Testing Contract |
| A package manager or toolchain pin changes | Tooling Lock |
| A known gap is fixed | Delete its Known Gaps line |
| Every command above was run and passed | Last reviewed |
```

A command whose obvious form fails quietly gets one line beneath it: a flag that
is not optional, an invocation that resolves imports differently, a step CI runs
that is easy to skip locally. Name what breaks and how it presents.

The Keep docs current table is shared. A table already present keeps its rows;
add the four above where missing. Drop a row whose section you deleted.

General behavioral rules stay out: the reading agent has its own global
configuration, and a copy goes stale when that changes. Repo worked by several
agent tools, by agents whose config the user does not control, or the user asks
→ add a Baseline Rules block per
[references/baseline-rules.md](references/baseline-rules.md).

## Phase 3: Companion files

Claude Code reads `CLAUDE.md`, not `AGENTS.md`. Write a `CLAUDE.md` holding
exactly one line, `@AGENTS.md`, whenever none exists, whichever harness runs
this. An existing `CLAUDE.md` with project facts → propose moving them into
`AGENTS.md` and leaving the import line; the user decides.

Other tools get a file only when the repo already has one or the user asks. It
holds a pointer and nothing else, and names no path that exists on one machine
only:

| Tool | File |
|------|------|
| Gemini | `GEMINI.md` |
| Cursor | `.cursor/rules/project.mdc` |
| Copilot | `.github/copilot-instructions.md` |

```md
> Project commands, context, and invariants: `./AGENTS.md`
```

## Generation rules

1. **Evidence only.** Every command, version, policy and owner traces to a file.
   No evidence → the line stays out and the gap goes in the final report.
2. **Commands are traced, not tagged.** Each command in the file matches the
   script, target or CI step it came from. A command you could not trace goes in
   the report, never into the file with an "unverified" or "inferred" tag:
   evidence tags load every session to serve an audit that happens rarely.
   Commands are read, not run; the report lists each with its source as "not
   run" and offers to run the read-only ones (lint, typecheck, unit tests).
3. **Package manager by lockfile:** `pnpm-lock.yaml`→pnpm · `yarn.lock`→yarn ·
   `package-lock.json`→npm · `bun.lockb`/`bun.lock`→bun · `uv.lock`→`uv run` ·
   `poetry.lock`→`poetry run` · `Pipfile.lock`→`pipenv run`. Two lockfiles for
   one ecosystem → say so and ask.
4. **Progressive disclosure.** Inline what every task needs: commands,
   invariants, the doc map. Material only some tasks reach (a deploy procedure,
   a long list of critical flows, a migration guide) goes behind a `Read when`
   pointer to the doc that holds it. No doc holds it → inline it if it fits in a
   few lines, otherwise name the proposed doc in the report. Each `Read when`
   line leads with the task that triggers it, one task per line: that wording
   decides whether an agent ever opens the doc.
5. **One copy of each fact.** `AGENTS.md` holds the project facts. Companion
   files hold a pointer. A nested `AGENTS.md` holds only its difference.
6. **Every line earns its load.** The file loads every session. Cut a line that
   restates what the agent would do anyway or that one lookup answers. Keep what
   looking cannot find: the unwritten convention, the reason behind a choice,
   the gotcha no config confesses.
7. **Current state only.** Git holds the history; the project's roadmap holds
   status. A line recording that something was built, probed or amended belongs
   in a commit or the roadmap.
8. **Report at the end:** files changed, lines cut by the removal test,
   unresolved gaps, and each command with its source.

## Completion criteria

- Every line is project-specific and traced to evidence.
- Every line passed the removal test: deleting it would cause an agent to make a
  mistake.
- Everything inline is needed by every task; the rest sits behind a `Read when`
  pointer or is named in the report as a proposed doc.
- A `CLAUDE.md` imports `AGENTS.md`, and Keep docs current has a row for every
  section this run wrote.
- Other skills' sections and rows are intact.
- No existing instruction file changed without approval.
