# Matt Pocock's skills: workflow, composition and drift from the han-* adaptations

Retrieved 2026-11-23. Input to deciding what hanseo-skills should borrow from, or resync with, `mattpocock/skills`. Findings in sections 1-3 describe Matt's skills. Sections 4-6 judge hanseo against them and are marked as judgement.

## How to read this file

- **[source]**: stated in a file I read. Paths are given.
- **[git]**: from commit history (`gh api repos/mattpocock/skills/commits`, `git log` in this repo).
- ***inferred***: my reasoning, not the source's statement. **inconclusive**: the check could not settle it.

Sources:

- Local install: `~/.agents/skills/<name>/` (symlinked from `~/.claude/skills/`). Provenance from `~/.agents/.skill-lock.json`: 28 skills have `source: mattpocock/skills`, last updated 2026-11-04 (retro, teach, to-questionnaire, wait-what, pr: 2026-10-06). SKILL.md files carry no author field; the lock file is the provenance record. `brainstorming`, `executing-plans`, `systematic-debugging`, `test-driven-development`, `writing-plans` are `obra/superpowers`, not Matt's.
- Upstream: shallow clone of `github.com/mattpocock/skills` at `abeeaf9` (2026-11-19). Paths below like `skills/engineering/to-spec/SKILL.md` are upstream paths.
- **Local install is current.** Every upstream `engineering/` and `productivity/` SKILL.md and sibling .md is byte-identical to the local copy except `teach/SKILL.md` (two small wording fixes from `ad5400b`, 2026-10-07). Commits after 2026-11-04 touch only `in-progress/software-factory` and CI frontmatter checks [git]. So "upstream" and "local install" are interchangeable below.

---

## 1. The end-to-end workflow

The canonical map is the router `ask-matt/SKILL.md` plus `ask-matt/PHASE-BOUNDARIES.md` [source].

### Main flow: idea → ship

| Step | Skill | Hands off |
|---|---|---|
| 0 (once per repo) | `/setup-matt-pocock-skills` | Issue tracker config, triage label strings, domain doc layout (`GLOSSARY.md` + `docs/adr/`) |
| 1 | `/grill-with-docs` (in a repo) or `/grill-me` (no repo) | Shared understanding in the conversation; `GLOSSARY.md` and ADRs written inline (grill-with-docs only) |
| 2 (branch) | `/handoff` → `/prototype` in a fresh session → `/handoff` back | A question that needs a runnable answer. Prototype kept on a `prototype/<name>` branch as a primary source |
| 3a multi-session | `/to-spec` → `/to-tickets` | Spec published to the tracker with `ready-for-agent`; tickets as tracer-bullet slices with blocking edges (one file per ticket under `.scratch/<feature>/issues/` locally, native blocking links on GitHub) |
| 3b build | `/implement` per ticket, `/clear` between each; or `/implement-spec` for the whole graph | `/implement` runs `tdd` + `code-review` per ticket and commits. `/implement-spec` runs implementer subagents across the frontier on one integration branch, one `code-review` at the end |
| 3c single-session | `/implement` in the same context | |
| PR | `pr` (model-invoked) | PR body: smallest visual, before/after evidence, one-way/two-way door |
| 4 | `/retro` | Environment changes (navigation pointers, checks, coding standards, steering files), not code |

Context hygiene rule [source, ask-matt]: keep steps 1-3 in **one unbroken context window** until after `/to-tickets`; each `/implement` starts fresh. The limit is the ~150k-token "smart zone"; if it approaches, `/compact` at a phase boundary.

### On-ramps that merge onto the main flow [source, ask-matt]

- `/triage`: inbound issues you did not create → agent-ready issues for `/implement`. Never triage `/to-tickets` output.
- `diagnosing-bugs`: hard bugs; then `/retro` in the same session; a missing seam goes to `/improve-codebase-architecture`.
- `/wayfinder`: an effort too big and foggy for one session. Produces decisions, not deliverables. When the map clears it hands off at `/to-spec`, never straight to `/implement` unless the effort turned out small.
- `/improve-codebase-architecture`: upkeep; a picked candidate becomes an idea for `/grill-with-docs`.

### Phase boundaries (`PHASE-BOUNDARIES.md`) [source]

At a boundary, the first yes wins: (1) **Continue** if the next phase needs this one as a primary source or the window has room; (2) **`/clear`** if the context is disposable; (3) **`/handoff`** only for a new harness, new directory, a colleague, or a mid-phase side task; (4) **subagent** if the task can run AFK; (5) otherwise **`/compact`** with an instruction. Every move except Continue turns a primary source into a lossy secondary one.

### Invocation per skill [source: frontmatter + `agents/openai.yaml`]

User-invoked = `disable-model-invocation: true` **and** `policy.allow_implicit_invocation: false`. Every Matt skill has both or neither.

- **User-invoked**: ask-matt, grill-me, grill-with-docs, handoff, implement, implement-spec, improve-codebase-architecture, retro, setup-matt-pocock-skills, teach, to-questionnaire, to-spec, to-tickets, triage, wait-what, wayfinder.
- **Model-invoked**: grilling, domain-modeling, codebase-design, tdd, code-review, diagnosing-bugs, prototype, research, pr, wizard, writing-for-agents, resolving-merge-conflicts.

Pattern: user-invoked skills orchestrate; model-invoked skills hold reusable discipline or vocabulary (README "Reference" section) [source].

## 2. How the skills compose

### Mechanism [source: upstream `.agents/invocation.md`]

- An operative dependency is written as an explicit tool instruction: `Call the Skill tool with "grilling"`. Not a `../other/FILE.md` link and not a bare `/skill` mention. Stated reason: naming the tool "gets a higher hit rate than dropping a `/name` into prose"; dropping the `/` keeps it harness-neutral.
- Two skills = two calls, worded `Call the Skill tool twice, for "grilling" and "domain-modeling"` (commit `447ca70`) [git].
- Router prose (ask-matt, bucket READMEs) names skills with `/name` as plain labels; it invokes nothing.
- Shared reference lives inside the skill that owns it; others reach it by calling that skill.

### Every composition edge in the installed skills [source: grep of "Skill tool"]

| Caller (invocation) | Calls |
|---|---|
| grill-me (user) | grilling |
| grill-with-docs (user) | grilling, domain-modeling (whole skill body is that one line) |
| triage (user) | grilling + domain-modeling, "if the request needs fleshing out" |
| wayfinder (user) | grilling + domain-modeling (destination, grilling tickets, default); research (AFK tickets, in parallel subagents); prototype |
| improve-codebase-architecture (user) | codebase-design, grilling, domain-modeling |
| implement (user) | tdd, code-review |
| implement-spec (user) | tdd (inside each implementer subagent), code-review |
| retro (user) | writing-for-agents (always, commit `8267225`) |
| tdd (model) | codebase-design, "a reference to consult, not a session to run" |
| handoff (user) | none; tells the *next* agent which skills to call |

### Does any skill invoke a user-invoked skill? No.

- Invariant, verbatim [source, `.agents/invocation.md`]: "A user-invoked skill may invoke model-invoked skills, but it can never reach another user-invoked skill." Repeated in README "Reference".
- Every Skill-tool target above is model-invoked. Verified against the frontmatter list in section 1.
- A user-invoked precondition is phrased as an instruction to the human: "tell the user to run `/setup-matt-pocock-skills`" (to-spec, to-tickets, implement-spec, code-review, triage, wayfinder). Commit `1dab982` "Stop skills from calling other user-invoked skills" (2026-08-15) [git].
- That pointer appears only in hard-dependency skills (ADR `.agents/adr/0001-explicit-setup-pointer-only-for-hard-dependencies.md`). Soft-dependency skills (tdd, diagnosing-bugs) refer vaguely to "the project's domain glossary" and degrade gracefully.
- Sequencing between user-invoked steps (to-spec → to-tickets → implement) is carried only by the router and the human. No skill's closing step names the next one; ask-matt is the only place the chain is written down.
- Subagents start "with a fresh context window, not a fork" (commit `7739eae`, 2026-10-29: grilling, code-review, implement-spec, research) [git].

## 3. Writing principles (brief; `writing-for-agents/SKILL.md`, `SKILL-MECHANICS.md`)

- **Context pointer**: a reference that names out-of-context material and the condition for reaching it. Its wording decides reliability. Front-load the leading word, one trigger per branch, cut identity the body carries.
- **Two loads**: context load (always-loaded tokens) vs cognitive load (the human as index).
- **Information hierarchy**: in-file step → in-file reference → disclosed reference behind a pointer. Disclose what only some branches need. Co-locate a concept's rules.
- **Completion criteria**: clarity (done vs not-done) and demand (how much it requires). Fix premature completion by sharpening the bound first; hide later steps only across a real context boundary.
- **Leading words**: a pretrained concept repeated as a token (*tight*, *red*, *tracer bullets*, *fog of war*). Prompt the positive; negation primes the banned behaviour.
- **Pruning**: single source of truth; the environment is a source of truth and restating it is a cache; hunt no-ops sentence by sentence.
- **Invocation** (`SKILL-MECHANICS.md`): user-invoked descriptions are human-facing one-liners with trigger lists stripped. Pick model-invocation only when the agent or another skill must reach it. Reference shared by two user-invoked skills lives in a plain file, since neither can fire the other. A **router skill** cures piled-up cognitive load; it can only hint.
- Retro adds an operational split [source, `retro/SKILL.md`]: the reviewer agent, not the implementer, enforces coding standards. Mechanical violations become deterministic checks; only judgement calls go in `CODING_STANDARDS.md`.

---

## 4. Matt skills or ideas with no hanseo counterpart

Judgement, not source. "Covered" means a hanseo skill already does the job.

| Matt skill | What it does [source] | hanseo coverage | Worth adding? (*inferred*) |
|---|---|---|---|
| ask-matt + PHASE-BOUNDARIES | User-invoked router over the flow; five-option boundary tree | Partial: groundwork's owner table (`groundwork/SKILL.md` "Owners"), next-step lines at the end of han-grill and han-to-spec | **Medium.** hanseo has 11 skills, mixing Matt-adapted and original ones, and the chain lives only in skill closings. A short "Workflow" section in the root README costs no context load. A router skill pays off only if the user forgets which skill comes next. |
| wayfinder | Foggy multi-session effort as a map of decision tickets (research/prototype/grilling/task), claimed per session, fog of war, out of scope | Largely covered by docket (destination, slices, "Still unclear", roadmap as an index, commit `528e317`) | **No new skill.** One idea to borrow: wayfinder fires AFK research questions as parallel `research` subagents at charting time. docket could do the same for questions that need outside facts. |
| handoff | Portable summary file in OS temp, with "suggested skills", no duplication of existing artifacts | Not covered. docket's carried instructions and han-to-spec handle durability through repo artifacts instead | **No.** Matt's own boundary tree narrows handoff to portability cases. It composes with hanseo as-is. |
| retro | Session retrospective → environment improvements, ranked by severity | Not covered | **Low cost, real value.** Use Matt's as-is. Optional: stint and han-implement-spec reports could suggest `/retro` when a run went sideways (Matt's diagnosing-bugs on-ramp does this). |
| prototype | Throwaway code answering one design question; kept on a `prototype/<name>` branch | han-to-spec and han-to-tickets keep the prototype-snippet exception. han-grill never routes to a prototype | **Small improvement:** han-grill's "Decision help" could name `/prototype` when a question needs a runnable answer (Matt's main-flow step 2). |
| to-questionnaire | Grills the user about the *send* and writes a questionnaire for the person who holds the answer | Not covered | **Small improvement:** han-grill's "Decision help" and product-discovery could point to it when the deciding fact sits with someone else (a customer, a colleague). |
| research | Background agent, primary sources, cited Markdown in the repo | `.agents/research/` notes exist; no skill | No. Matt's composes. |
| triage | State machine of triage labels for inbound issues/PRs; `.out-of-scope/` knowledge base | None | No. Outside hanseo's scope (inbound work, not designed work). |
| wait-what | Re-pitch the last message in ASD-STE100 Simplified Technical English using `GLOSSARY.md` | None | No. Note: it names `GLOSSARY.md`/`GLOSSARY-MAP.md` only, so in a repo whose glossary is `CONTEXT.md` (this one) it misses the vocabulary (*inferred*). |
| teach, wizard, pr | Learning workspace; human-only setup script; PR body shape | None | No. Unrelated to hanseo's gaps; pr is model-invoked and fires anyway. |
| improve-codebase-architecture, codebase-design | Deepening survey; deep-module vocabulary | han-grill uses codebase-design vocabulary "when installed" | Covered by reference. |
| In-progress (beta, not in plugin): chief-of-staff, loop-me, claude-handoff, software-factory, setup-ts-deep-modules, writing-* | `skills/in-progress/README.md` | n/a | Watch only. chief-of-staff (long-running goal coordinated through subagents and a wiki) overlaps docket's territory. |

Upstream also records rejected ideas in `.out-of-scope/` (e.g. `subagent-recursion.md`: depth guards belong to the harness, not skill prose; `new-skills.md`: "if the behaviour composes from existing skills, it doesn't get a new one"). The same bar is a useful filter for any han-* addition (*inferred*).

---

## 5. Drift: han-* adaptations vs current upstream

Upstream change dates from per-path commit history [git]. hanseo adaptations were written 2026-10-20 to 2026-10-23 (`efc82ad`, `c7efd4e`).

### han-grill vs grilling

- **Wrong claim in the READMEs.** `skills/han-grill/README.md:39` says "Rounds, not one question at a time … Matt's `grill-me` asks one at a time". `skills/han-grill-with-docs/README.md:41` says "not Matt's one-question flow". The root README lists "numbered rounds of independent questions" as a difference. Upstream grilling has worked in numbered frontier rounds since 2026-07-31 (`bfdaef8`; content at `85f83d3`, 2026-08-20, verified). It was already true when the README says Matt's main was inspected (2026-10-20). Rounds are a shared trait, not a difference.
- Upstream "word each question so 'yes' accepts your recommended answer" (`95249b0`, 2026-10-07): han-grill has it ("Make the recommended action explicit so 'yes' can accept it"). No drift.
- Upstream "dispatch a sub-agent with a fresh context window" for facts (`7739eae`, 2026-10-29, after the adaptation). han-grill says "independent fact-finding subagents", without "fresh context window". Minor; whether "independent" excludes a forked context is **inconclusive**.
- Upstream separates questions in a round with `---` (`85f83d3`). han-grill's template shows one question. Cosmetic.
- Deliberate differences, not drift: han-grill finishes when the "consequential frontier" is resolved, while Matt's ends when "the frontier is empty … nothing left silently assumed". han-grill adds costed options, approach-first, structure probes and a next step.

### han-grill-with-docs vs grill-with-docs

- No upstream behaviour drift. Upstream renamed `CONTEXT.md` to `GLOSSARY.md` (`d80fa0f`, 2026-09-17) and setup now reads legacy `CONTEXT.md` (`ed0de59`). han-grill-with-docs already searches `CONTEXT.md`, `GLOSSARY.md` and `GLOSSARY-MAP.md`.
- **Invocation mismatch (hanseo-side bug).** `676db6c` (2026-11-04) removed `disable-model-invocation` from `han-grill/SKILL.md` so han-grill-with-docs could call it. `skills/han-grill/agents/openai.yaml` still has `policy.allow_implicit_invocation: false`. Under Matt's rule ("Keep the two in sync: a skill is user-invoked in both harnesses or neither"), han-grill is still user-invoked in Codex, so `/han-grill-with-docs` there cannot reach it (*inferred* from the documented semantics; not run in Codex).
- The README's "one-question flow" claim is wrong (see above).

### han-to-spec vs to-spec

- to-spec is unchanged since 2026-08-19 [git], before the adaptation. No upstream drift.
- Remaining differences, apparently deliberate: to-spec applies the `ready-for-agent` label on publish; han-to-spec has no label step and uses Draft/Ready readiness. Matt's spec template forbids file paths; han-to-spec names modules and contracts. Neither is flagged in the README.

### han-to-tickets vs to-tickets

- Upstream changes from 2026-10-06 (`cffab50`, `9e2abf8`: tickets become **sub-issues** of a source issue; never modify the parent) predate the adaptation and are mostly carried over: "preserve source-parent links … Do not edit or close the parent", plus `ready-for-agent`, expand–migrate–contract, and one local file per ticket. Small gap: Matt says "make each ticket its sub-issue (tracker doc's operation)", while han-to-tickets says "preserve source-parent links" without naming the sub-issue operation (*inferred* to produce a plain link on GitHub).
- No later upstream changes.

### han-implement-spec vs implement-spec

- Upstream after the adaptation: `a496f55` "a fresh implementer subagent per ticket" and `7739eae` "fresh context window, not a fork" (both 2026-10-29). han-implement-spec got the same idea on its own (`71dba89`, 2026-10-23: "A fresh agent per ticket … no ticket inherits another's context"). No drift. Adding the words "fresh context window" would match upstream exactly.
- The README "Differences" table matches the current upstream text: merger subagent per result, optional exploration subagent, one final review with all findings fixed, implementer merges the integration tip before reporting.

### Cross-cutting convention gaps (hanseo vs Matt's `.agents/invocation.md`)

- **Invoking dependencies.** hanseo's `.agents/invocation.md` says to reference skills "by slash command name in prose". stint (`run /tdd`, `run /code-review`) and han-implement-spec (`Builds with /tdd`, `/code-review`) do that. Matt's convention is `Call the Skill tool with "tdd"`, for the hit-rate reason above [source]. han-grill-with-docs already uses Matt's form. That makes two conventions in one repo.
- **The user-invoked → user-invoked invariant** is absent from hanseo's `.agents/invocation.md`, though hanseo follows it in practice (han-grill and han-to-spec end with "The user runs that step").
- **openai.yaml sync.** charter, docket, stint and product-discovery have no `agents/openai.yaml`. docket and stint are user-invoked in Claude Code only (*inferred*: Codex allows implicit invocation when there is no policy).
- **charter**: the root README says user-invoked; `skills/charter/SKILL.md` has no `disable-model-invocation`, and its description carries trigger phrases, so it is model-invoked.
- **stint** is user-invoked but its description is a model-facing trigger list ("Use when … 'implement this' …"). Matt's rule: user-invoked descriptions are one-line human summaries, since no model reads them for triggering.

---

## 6. Most applicable, ranked (*inferred*)

1. Fix `skills/han-grill/agents/openai.yaml` (drop the `policy` block) so han-grill is model-invoked in Codex too, matching `676db6c`'s intent.
2. Correct the "Matt asks one at a time" claim in `skills/han-grill/README.md`, `skills/han-grill-with-docs/README.md` and the root README's han-grill bullet. Rounds are shared with Matt's grilling.
3. Pick one convention for operative dependencies. Matt's `Call the Skill tool with "x"` is documented as more reliable. Apply it to stint and han-implement-spec, or record why hanseo keeps `/x`.
4. Add the user-invoked → user-invoked invariant and the SKILL.md/openai.yaml sync rule to `.agents/invocation.md`. Reconcile charter's README classification with its frontmatter.
5. Small borrowings: han-grill "Decision help" can name `/prototype` or `/to-questionnaire`; docket can fire research questions as parallel subagents (wayfinder step 5); stint and han-implement-spec reports can suggest `/retro` after a run that went sideways.
6. No new han-* skill is clearly warranted. wayfinder is mostly covered by docket; handoff, retro, research and triage compose unchanged.
