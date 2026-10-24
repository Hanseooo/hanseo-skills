# groundwork

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Set up the docs an agent needs to build a software project well, or fill the
gaps in an existing repo's. It writes a project brief, a project roadmap and
`AGENTS.md`, and plans architecture and design as their own focused sessions.

## Where this sits

At the start of a project, or whenever an existing repo's docs have stopped
helping agents. groundwork writes only the docs no other skill owns, and routes
the rest:

- commands, tooling and the testing contract → `/project-init`
- architecture and ADRs → `/charter`
- a large milestone's slices → `/docket`
- slice specs → `/han-to-spec` or your spec skill
- design and brief interviews → `/han-grill` or your grilling skill
- the glossary → domain-modeling

It works without them: a missing companion shows up in the roadmap as "needs
<skill>", never as a stand-in doc. The audit-then-confirm setup flow follows
Matt Pocock's `setup-matt-pocock-skills`. The milestone map with typed sessions
borrows from his **wayfinder**.

## The problem

Asked to "set up the docs", an agent writes all of them in one sitting: PRD,
master spec, roadmap, architecture, design and glossary. They sound finished,
but the architecture was written before a stack was chosen, and the design
doc before anyone discussed the UI. The master spec and the roadmap both claim
scope.

Real projects show where that leads. In one, a "canonical" master spec sat next
to a 1,000-line PRD and a 1,300-line tech spec, and the pricing rule still said
"must match PRD". In another, phases were repeated in the roadmap, master spec
and a handoff doc, and the handoff doc slowly turned into a status log. Entry
docs went stale within weeks. They pointed at files the code had renamed and
listed no test command, even though e2e tests existed.

Asked to clean up such a repo, a baseline agent read the master spec's claim to
be "the one reference, as it says" and proposed merging the other specs into it.

## How groundwork solves it

**Every fact gets one owner.** A table maps each question to the doc that
answers it: where the project is going, in what order, how to run it, how it is
built, how the UI behaves. Every other doc links to the owner. A master spec has
no question left to answer, so it never gets written.

**Deep docs get their own sessions.** Intake writes only the brief, the roadmap
and `AGENTS.md`. Architecture, design and any doc consolidation become entries
in milestone M0 of the roadmap. Each one runs in a fresh session with a
kickoff prompt, so each topic gets full attention, and M0 completes like any
other milestone instead of leaving a setup file behind.

**Docs stay tied to the code.** Commands come from `/project-init`, which records
only what the scripts and CI show. Versions and file lists stay in config. A doc that contradicts
the code is reported with both sides cited. A short event table in `AGENTS.md`
says which doc to update when a slice finishes, a decision is made or a command
changes.

**The roadmap feeds docket.** Each milestone holds an outcome, exit criteria,
dependencies, project rules and exclusions, which is exactly the destination
`/docket` starts from. The IDs never collide: `M` and `P` at the project
level, docket's own D, RS and J inside its files.

## When not to use it

- **A quick script or throwaway prototype.** A README line on how to run it is
  enough.
- **You only need architecture decisions or agent guardrails.** Run `/charter`.
- **You only need one feature planned.** Run `/docket`.
- **The docs work and you want code-health advice.** Use an audit skill such as
  `/improve`.
- **A project with no code, such as a course or a docs-only repo.** The commands,
  check loop and architecture parts do not apply.

## Install

Via the [skills.sh](https://www.skills.sh) CLI (works for Claude Code and other
supported agents):

```
npx skills add Hanseooo/hanseo-skills
```

Select `groundwork` when choosing skills, or install the whole collection.

Or clone the collection and reference this skill's folder directly:

```
git clone https://github.com/Hanseooo/hanseo-skills
```

- **Claude Code:** clone into `~/.claude/skills/groundwork` (use
  `skills/groundwork/` as the skill root).
- **Codex, opencode, Antigravity CLI, or others:** clone anywhere and load
  `skills/groundwork/SKILL.md` per that tool's own custom-instructions/skill
  mechanism, or paste `SKILL.md`'s contents into the session.

Update later with `git pull` (or re-run `npx skills add`).

Companions, all optional: `/project-init` (commands and testing contract),
`/charter` (architecture), `/docket` (milestone
breakdown), `/han-grill` or another grilling skill (design and brief
interviews), domain-modeling (glossary). Without a grilling skill, the design
session stops at its agenda.

## Use

**New project:**

```
/groundwork a CLI that syncs Markdown notes to a static site
```

**Existing repo:**

```
/groundwork our agent docs are a mess, fill what's missing
```

**Design session, from M0:**

```
/groundwork design
```

Intake reads the repo, settles the destination with you, and says which docs
the project needs and why. In an existing repo it first audits each doc as
present, stale, competing, unrouted or missing. You see every draft in one
message before anything is written. It ends by naming M0's first session and
the commands to start it. Run `/groundwork` again later and it resumes M0
instead of starting over.

## FAQ

### How is this different from `/charter`?

charter owns architecture, ADRs and the operating contract, and refuses to
scaffold. groundwork owns the brief, the project roadmap and `AGENTS.md`'s
routing, and schedules charter as an M0 session. Need only the decision layer?
Run charter alone.

### How is this different from `setup-matt-pocock-skills`?

Matt's setup configures a repo for his skills: the issue tracker, triage labels
and where domain docs live. groundwork sets up the project's own docs. They
don't conflict, and you can run both.

### Why no PRD or master spec?

The brief does the PRD's job without assuming the project is a product. A master
spec would claim scope that already belongs to the brief, the roadmap and the
slice specs, which is the duplication behind the stale docs above. Its
cross-cutting rules become `P-` rules in the brief.

## What it deliberately is not

- A code scaffolder: no folders, boilerplate or config files.
- A spec or ticket writer.
- A doc migrator: changing a repo's layout is an M0 session you choose.
- Background automation: docs stay current through the event table, read by
  whichever session is working.

## License

[MIT](LICENSE)
