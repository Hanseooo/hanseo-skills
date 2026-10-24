---
name: groundwork
description: Set up a software project's agent docs, or fill the gaps in an existing repo's, and plan the deep sessions for architecture and design.
disable-model-invocation: true
---

# groundwork

Lay the groundwork an agent needs to build a software project well, whatever
its stack or kind: a **project brief** that says where the project is going, a
**project roadmap** that says how far it has got, and an `AGENTS.md` that says
where each doc lives and when to update it. Deeper docs are planned as **M0** sessions, each run
in a fresh session, rather than written shallow now.

## Paths

- `/groundwork [idea or context]` → **intake**, below. In a repo with existing
  docs, intake starts with the audit in [references/audit.md](references/audit.md).
- `/groundwork design` → the design path in
  [references/design.md](references/design.md).
- A project roadmap with an M0 milestone already exists → **resume**: reconcile
  M0 against what is on disk, mark finished sessions with links, name the next
  one. Update that roadmap; one roadmap per project.

## Owners

Every fact has exactly one **owner** doc. Anywhere else, link to the owner.

| Question | Owner | Write it when |
|---|---|---|
| Who is this for, what does done look like, what is out? | Project brief | Always |
| In what order, and how far along? | Project roadmap | Always |
| Which doc answers what, and what do I update when? | `AGENTS.md`: Documentation Map and Keep docs current | Always |
| How do I install, run and test, and what must not break? | `AGENTS.md`: commands, tooling, testing contract and invariants, via `/project-init` | Code exists |
| How does one milestone split into slices? | `/docket` | The milestone needs several specs |
| Exactly how does one slice behave? | `/han-to-spec` or the user's spec skill | Per slice |
| How is the system built, and why? | `architecture.md` and ADRs, via `/charter` | Two or more modules, or an external service |
| How does the UI look and behave? | `design.md`, via `/groundwork design` | A visual UI |
| What do our words mean? | Glossary, via domain-modeling | Domain terms appear |

A master spec has no question left to own, so groundwork writes none. Detail a
roadmap would carry belongs to the milestone's docket or spec.

## Guardrails

These bound what gets written, what counts as done and what needs the user's
confirmation. How to reason, what to ask and which sections fit stay open.

1. Before writing a fact, find its owner. Another doc owns it → link.
2. Commands belong to `/project-init`. Intake leaves an existing Commands
   section exactly as it is, wrong lines included, and lists each problem in
   the report and in the `/project-init` session's kickoff.
3. An unanswered question becomes an M0 entry or a brief open question. Prose
   states only what is settled.
4. A doc whose trigger is unmet stays unwritten.
5. Versions, counts and file lists stay in config and git; docs point there.
6. Code wins. A doc that contradicts the code is reported with both sides cited.
7. Show drafts first, then write. Existing files are amended in place;
   removing a file is the user's call, made separately.

## Intake

1. **Inventory.** Read the README, `AGENTS.md`, `CLAUDE.md`, `docs/`,
   `planning/`, ADRs, any glossary, the manifest, scripts and CI config. Existing
   docs → run the audit too. Done when you can say, per owner row, which doc
   holds it today or that none does.
2. **Destination.** Settle who the project serves, what done looks like, what is
   in and out, and rules that span the work. Ask only what the evidence leaves
   open; for a real choice, offer options with tradeoffs and recommend where
   evidence favors one. Investigate discoverable facts yourself, dispatching
   research subagents where available. Still no nameable destination → the
   brief becomes M0's first session, a grilling session, and intake writes only
   the roadmap and `AGENTS.md`.
3. **Pick docs.** For each owner row, state whether its trigger is met and the
   evidence. The user confirms. Paths follow the repo's conventions; with none,
   rarely-changing docs go in `docs/` and route and status docs in `planning/`.
4. **Draft.** Load the reference for each doc you write:
   [brief](references/brief.md), [roadmap](references/roadmap.md),
   [AGENTS.md](references/agents-md.md). Show every draft or diff in one
   message and take one confirmation, however hurried the user is.
5. **Write**, then hand off: name M0's first session and the exact commands to
   start it, and stop.

Intake is done when every confirmed doc is written or is an M0 entry, every
command problem the audit found is reported, and every open question has a home.

## M0 sessions

M0, "Project foundations", is the first milestone in the project roadmap. Each
entry is one fresh session with a kickoff prompt (shape in
[references/roadmap.md](references/roadmap.md)):

- **Architecture** → `/charter`. Where `CLAUDE.md` only imports `AGENTS.md`, the
  kickoff tells charter to write its contract section into `AGENTS.md`.
- **Design** → `/groundwork design`, then the user's grilling skill
  (`/han-grill`) on the agenda it lists, then "write it", all in one session.
- **Brief**, when intake could not name a destination → the grilling skill,
  then `/groundwork` again to write the brief.
- **Consolidation**, when the audit finds docs competing for one owner row.
- **Repo contract** → `/project-init`: commands, tooling, testing contract
  and invariants. First in M0 when code exists; otherwise once the stack exists
  and the check loop can run.

Name each session's companion skill by command. User-invoked skills fire only
when the user types them. A companion that is not installed → the entry reads
"needs <skill>", and groundwork leaves that doc to it.
