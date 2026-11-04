---
name: project-init
description: Write or audit a repository's AGENTS.md from repo evidence and a short interview - commands, run steps, testing contract, invariants and scope. Usage: /project-init [project context]
disable-model-invocation: true
---

# project-init

Write or audit a repository's `AGENTS.md`. The repo supplies what it can show;
a short interview supplies what it can't. Project instructions hold
repository-specific guidance; general behavior stays global.

Where this says "ask", use the harness's question tool. Where it says "inspect",
use read-only file tools (glob, read, search). Some setups allow no shell, and a
failed shell call can abort the run partway through.

## Phase 0: Safety check

Locate the repository root. No version control → treat the working directory as
the root and say so in the final report. From the root down to the working
directory, check for agent instruction files: `AGENTS.override.md`, `AGENTS.md`,
`CLAUDE.md`, `CLAUDE.local.md`, `GEMINI.md`, `.cursor/rules/`, `.cursorrules`,
`.github/copilot-instructions.md`, `.windsurfrules`.

- **No `AGENTS.md` or `CLAUDE.md`** → Phase 1. Another tool's file is input, and
  stays as it is.
- **Either exists** → read it and run Phase 1, then audit it per
  [references/audit.md](references/audit.md). Propose a diff and settle patch,
  replace or stop. A choice already in the user's message stands. Stop means stop.

Sections other owners write survive every patch and replace, word for word: a
delimited block (`<!-- charter:begin -->` … `<!-- charter:end -->`), a section
another skill or tool wrote (such as `## Agent skills`), Documentation Map rows
pointing at docs that exist, and Keep docs current rows. A broken pointer inside
one goes in the report. A placeholder line such as "Commands arrive with M0's
repo contract session" is yours to replace.

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
- Run steps: the dev command, its URL or port, and where a test login comes from.
- Docs: `README*`, `docs/**/*.md`, root `*.md`, and any architecture, design,
  spec, brief or roadmap doc.
- Test layout, fixtures, and nested agent instruction files.
- Environment pinning: virtualenv directories, `.tool-versions`, `.nvmrc`,
  container and devcontainer definitions.
- Patterns in migration: a deprecation marker, or two implementations of one job.
- Duplicate trees: `.worktrees/`, `.git/worktrees`, vendored or bundled copies.
  A repo-wide search hits every copy, and a hit inside one is another branch's
  state. Find them now, before any search you rely on.

Executable configuration, CI and code beat prose. Read enough to verify, not
everything.

## Phase 2: Interview

Build the list of open items. Each item would change a line in the file:

| Open item | When it is open | Ask as |
|---|---|---|
| What agents get wrong here: past breakage, unwritten rules | Always | Open question |
| What is out of scope right now | No brief or roadmap answers it | Open question |
| A command with two candidates the evidence can't separate | Evidence conflict | Choice |
| Which of two patterns new code uses | Two patterns and no marker | Choice |
| Whether this is a team repo | `.cursor/`, `.cursorrules`, `GEMINI.md`, `.windsurfrules` or Copilot instructions are present and the user hasn't said | Choice |
| Approval to run the checks in Phase 5, and to install first when dependencies are absent | Lint, typecheck, unit test or build scripts exist | Choice |

Drop any item the user's message or a doc already answers. Ask the rest together
in one round, numbered, each skippable. A choice uses this shape:

```md
**Q1 - <title>**: <the line this decides>

A. <option>
- Benefit: <concrete consequence>
- Cost: <concrete drawback>

B. <option>
- Benefit: <concrete consequence>
- Cost: <concrete drawback>

Recommendation: <option, and the evidence that favors it>
```

No evidence favors an option → say what would decide it instead. An answer can
open another item: ask a follow-up round only for that. Done when every item has
an answer, a skip, or a line in the report.

Things the repo should fix, such as a missing dependency or an unfinished
migration, go in the report and stay out of the interview.

No way to ask → list the questions in the final report and write what the repo
supports. A re-run with the answers fills the rest.

## Phase 3: Write AGENTS.md

`AGENTS.md` goes at the repository root. In a monorepo the root carries what is
true everywhere and names the packages; a package gets a nested `AGENTS.md` only
when its commands, package manager or test contract differ, and that file states
only the difference. Two independent projects sharing a repo with no root
tooling is not a monorepo: one root `AGENTS.md`, and every command names the
directory it runs from.

Include only sections that change agent behavior and that evidence supports.
Delete any section you cannot fill.

```md
# <Repo> Agents Configuration

Last reviewed: <only per Phase 5>

## Project Context
- Out of scope now: <what agents must not build yet, or a link to the doc that owns scope>
- Critical paths: <auth, billing, data pipelines, infra>, require extra review.

## Documentation Map
- Always read: <1-3 highest-signal docs>
- Read when: <task that needs it> → <doc path>

## Commands (Use Exactly)
- Install: <command>
- Run: <dev command> at <URL>. Test login: <where to get one>
- Lint: <command>
- Build/Typecheck: <command>
- Unit tests: <command>
- Integration/E2E: <command>
- Pre-merge verify: <ordered sequence>
- Single test: <the form a script or doc shows, or that ran in Phase 5>

## Tooling Lock
- Package manager: <name>
- Toolchain: <pinned invocation, such as `uv run` or the version in `.nvmrc`>

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
- In migration: <old pattern> → <new pattern>. New code uses the new one.

## Known Gaps
- <Open problems an agent would otherwise rediscover. Ask before fixing one, and
  delete its line in the same change.>

## Definition of Done
- <project-specific merge or PR requirements>
```

A command whose obvious form fails quietly gets one line beneath it: a flag that
is not optional, an invocation that resolves imports differently, a step CI runs
that is easy to skip locally. Name what breaks and how it presents.

A test login line says where a login comes from, such as a seed script.
Credentials stay out of the file.

Team repo → add a Baseline Rules block per
[references/baseline-rules.md](references/baseline-rules.md). Otherwise the
reading agent's global configuration supplies general rules.

## Phase 4: Companion files

Claude Code reads `AGENTS.md` on its own (v2.1.277 and later) when no `CLAUDE.md`,
`.claude/CLAUDE.md` or `CLAUDE.local.md` sits in the working directory or above.

- None of those → write no `CLAUDE.md`.
- One exists and lacks `@AGENTS.md` → `AGENTS.md` does not load. Say so in the
  report and propose adding that import line to it; the user decides. Propose
  moving project facts from it into `AGENTS.md` too.

Another tool gets a new file only when the user asks for it by name. It holds a
pointer and nothing else, and names no path that exists on one machine only. A
file that tool already has stays as it is; the report proposes adding the pointer.

| Tool | File |
|------|------|
| Gemini | `GEMINI.md` |
| Cursor | `.cursor/rules/project.mdc` |
| Copilot | `.github/copilot-instructions.md` |

```md
> Project commands, context, and invariants: `./AGENTS.md`
```

## Phase 5: Check and stamp

`Last reviewed` means the checks ran. The checks are the lint, typecheck, unit
test and build scripts; E2E and every other script stay unrun. With approval from
the interview:

1. Read the body of each check. One that touches a real service, a database or a
   deploy stays out, and the report names it.
2. Dependencies absent and installing approved → run the install command. The
   report names what it left behind.
3. Run the checks.
4. Single test built from the runner's usual form → run it on one existing test.
   A pass puts it in the file. Anything else leaves it in the report.
5. A check that fails gets a Known Gaps line with the failure it shows.
6. Every check reached the code → set `Last reviewed` to today.

Skipped, declined, or stopped by a missing tool or config → a new file has no
`Last reviewed` line, and an existing date stays as written. The report says the
checks did not run.

## Generation rules

1. **Evidence only.** Every line traces to a file or to the user's own words. The
   user saying it is enough; no repo file has to confirm it. No evidence → the
   line stays out and the gap goes in the report.
2. **Commands are traced, not tagged.** A line holds the command and any gotcha.
   Its source goes in the report, and so does any command you could not trace:
   evidence tags load every session to serve an audit that happens rarely. A form
   built from a tool's general usage is untraced until a run proves it.
3. **Package manager by lockfile:** `pnpm-lock.yaml`→pnpm · `yarn.lock`→yarn ·
   `package-lock.json`→npm · `bun.lockb`/`bun.lock`→bun · `uv.lock`→`uv run` ·
   `poetry.lock`→`poetry run` · `Pipfile.lock`→`pipenv run`. Two lockfiles for
   one ecosystem → an open item in the interview.
4. **Progressive disclosure.** Inline what every task needs: commands,
   invariants, the doc map. Material only some tasks reach (a deploy procedure,
   a long list of critical flows, a migration guide) goes behind a `Read when`
   pointer to the doc that holds it. No doc holds it → inline it if it fits in a
   few lines, otherwise name the proposed doc in the report. Each `Read when`
   line leads with the task that triggers it, one task per line: that wording
   decides whether an agent ever opens the doc.
5. **One copy of each fact.** `AGENTS.md` holds the project facts. Companion
   files hold a pointer. A nested `AGENTS.md` holds only its difference. A doc
   that owns scope gets a link.
6. **Every line earns its load.** The file loads every session. Cut a line that
   restates what the agent would do anyway or that one lookup answers, including
   an architecture overview the layout shows. Keep what looking cannot find: the
   unwritten convention, the reason behind a choice, the gotcha no config confesses.
7. **Current state only.** Git holds the history; the project's roadmap holds
   status. A line recording that something was built, probed or amended belongs
   in a commit or the roadmap.
8. **Report at the end:** files changed, lines cut by the removal test, unresolved
   gaps, unanswered questions, repo problems to fix, whether the checks ran, and
   each command with its source. Over 200 lines, or near Codex's 32 KiB combined
   limit → say so and name what could move behind a pointer.

## Completion criteria

- Every line is project-specific and traced to a file or the user's words.
- Every open item has an answer, a skip, or a line in the report.
- Every line passed the removal test: deleting it would cause an agent to make a
  mistake.
- Everything inline is needed by every task; the rest sits behind a `Read when`
  pointer or is named in the report as a proposed doc.
- `Last reviewed` is present only if every check reached the code, or as it stood
  before this run.
- Other owners' sections and rows are intact.
- No existing instruction file changed without approval.
