# pstack: what it is, how it composes skills, what hanseo-skills could take from it

Retrieved 2026-11-23. Input for distilling principles into hanseo-skills. It reports and compares. The fit column is a judgement against this repo's values, not a decision.

## How to read this file

- `pstack/<path>` means https://github.com/cursor/plugins/blob/7a583913eb1b8d1a0ee76753d1a2cf072e7b54f9/pstack/<path> (cursor/plugins at commit `7a58391`, 2026-11-21). Every pstack file cited was read from a clone of that commit.
- **inferred**: my reasoning, not the source's statement. **inconclusive**: the check could not settle it.
- Line counts come from `wc -l` on the clone.

---

## 1. Identification

"pstack" is **Lauren Tan's (GitHub/X handle poteto) Cursor plugin of agent skills**, shipped in Cursor's official plugin repo. The name is *inferred* to read "poteto stack", by analogy with gstack. The plugin calls its entry skill `poteto-mode`.

| Candidate | What it is | Evidence |
|---|---|---|
| **cursor/plugins/pstack** (upstream, most likely) | Original. `plugin.json` author "Lauren Tan", version 0.15.46, MIT. Ships 58 skills, 2 agents, 22 playbooks, a guide | `pstack/plugin.json`, `pstack/README.md` ("i'm poteto ... on the react core team") |
| backnotprop/pstack | Mirror of upstream for `npx skills add backnotprop/pstack`. Says everything after its header is the upstream README unchanged | https://github.com/backnotprop/pstack README, `MIRROR.md` |
| michael-denyer/pstack-claude | Port to Claude Code, Codex, Pi, Copilot. Pre-translates Cursor names and adds a SessionStart hook that injects routing to `poteto-mode` | https://github.com/michael-denyer/pstack-claude README, `plugins/pstack/hooks/hooks.json` |
| kashyab12/uni-pstack | Older portable port (last commit 2026-07-10), single `pstack/SKILL.md` entry plus references | https://github.com/kashyab12/uni-pstack README |

Not the same thing as Unix `pstack` (process stack dump) or Garry Tan's gstack.

Author's own announcement: search lists a Cursor page "pstack: the skills behind 10,000 agent runs" (`cursor.com/<locale>/lp-team/lauren`). Every fetch redirected to Cursor's pricing page, so its content is **inconclusive**. Secondary write-ups (flaviocopes.com/pstack) were not used as evidence.

Upstream claims it now installs in Claude Code directly: `/plugin marketplace add cursor/plugins`, `/plugin install pstack@cursor-plugins`, using a generated tree at `harness/pstack/claude-code` (`pstack/README.md` lines 29-40).

---

## 2. Workflow

### Entry and routing

One user-invoked router, `poteto-mode` (110 lines). Stated sequence (`pstack/README.md` "usage"; `pstack/skills/poteto-mode/SKILL.md` "Playbooks"):

1. Restate the goal and the smallest change that meets it, then proceed (SKILL.md line 19).
2. Match the task to one of 22 playbooks in `playbooks/<title>.md` and **copy its steps verbatim into the todo list** before working.
3. Each step names the skill it needs. "Non-negotiables" is a trigger table: code across a function boundary → `architect`, fan-out → `swarm`, bakeoff → `arena`, contested design → `interrogate`, prose → `unslop`, measured number → `benchmark-checklist`, long or unattended work → `show-me-your-work` (lines 21-29).
4. No playbook fits, or the work is large or unattended → `figure-it-out`, which designs a playbook first.
5. Every code playbook ends in **Opening a PR** (`playbooks/opening-a-pr.md`: "Invoked at the end of every other playbook").

There are **no planning skills by design**: "i don't believe in planning. the best spec is code" (`pstack/README.md` "why are there no planning skills?"). A plan exists only as the `multi-phase-plan` playbook.

### Two representative playbooks

| Feature (`playbooks/feature.md`) | Bug fix (`playbooks/bug-fix.md`) |
|---|---|
| 1 trace subsystem | 1 reproduce on the real surface yourself |
| 2 `architect` for the shape | 2 binary-search the cause, seeded by `why` (commit history); confirm the mechanism with runtime evidence |
| 3 "throughput checkpoint": 4 todo items (blocking steps, independent streams, shared state, smallest decomposition), `n/a: <reason>` rather than dropped | 3 `architect` if it crosses a function boundary, then delegate |
| 4 delegate code to a subagent with named data shape and success criteria, or `arena` when several shapes are valid | 4 verify on the same surface. "Inconclusive or wrong-surface is not a pass" |
| 5 verify on matching surface | 5 failing repro lands before the fix in history (`tdd`) |
| 6 small ordered commits (`sequence-verifiable-units`) | 6 Opening a PR |
| 7 `interrogate` if contested; 8 Opening a PR | |

### What each step hands to the next

| Artifact | Producer → consumer | Source |
|---|---|---|
| Todo list with playbook steps copied verbatim | router → itself, step by step | `poteto-mode/SKILL.md` "Playbooks" |
| Subagent brief: goal, file pointers not inlined context, explicit model per role | lead → `poteto-agent` delegates | "Subagents" section |
| Worktree or `/tmp/arena-<slug>/candidate-<n>/` per candidate | arena/swarm → judge and lead | `skills/arena/SKILL.md` Phase A.4 |
| `PASS` / `ISSUES` / `BLOCKED` report with evidence and exact SHAs | swarm workers → aggregator | `skills/swarm/SKILL.md` Phase B-C |
| Verified findings (command + output) | interrogate adversaries → user | `skills/interrogate/SKILL.md` |
| `decisions.tsv`, append-only | any long run → reviewer, cross-vendor auditor | `skills/show-me-your-work/SKILL.md` |
| Plan file with PR ids, dependency graph, per-PR checkboxes | `multi-phase-plan` → owner, on operator's explicit go | `playbooks/multi-phase-plan.md` |
| `wip:` commit + resume note (`/tmp/<slug>-resume.md`) | `pause-safely` → `session-pickup` | `playbooks/pause-safely.md`, `session-pickup.md` |
| PR body, ≤40 lines, fixed headings Why / What changed / Scope / Tradeoffs / Blast Radius / Verification | every playbook → reviewer | `playbooks/opening-a-pr.md`, `poteto-mode/worker.md` |

---

## 3. Design principles

### Size and structure

- 58 skills, 1,972 lines total. Leaf skills are tiny: `in-your-own-words` 7, `bro` 7, `why` 9, `architect` 11. Largest: `poteto-help` 162, `make-bot-ui` 115, `poteto-mode` 110.
- 24 **principle skills**, one principle each, 9-21 lines (`skills/principle-*/SKILL.md`). The router carries a one-line index of all 24, each phrased as *when it applies* plus the rule, and says "Read the leaf skill in full for any principle you apply" (`poteto-mode/SKILL.md` lines 31-72).
- Multi-agent skills are phase lists with letters (arena A-F, swarm A-D) and always begin "Open a todolist with one entry per phase before launching anything".
- Authoring rule: "When in doubt, delete. Keep only prose that changes a decision. Tell it to do the thing and skip the reason. Explain only when the rule is confusing without one ... Delegate to other skills by path. Don't restate." (`playbooks/authoring-a-skill.md` line 10).
- Every skill closes with a `**Reply:**` line naming what the final answer contains.

### Invocation model

- **57 of 58 skills set `disable-model-invocation: true`.** The only exception is `setup-pstack` (grep over `skills/*/SKILL.md`). So the user sees slash commands, the model never auto-fires a skill, and composition happens through the router reading files (section 5).
- Descriptions are still written as trigger lists ("Use for /arena, 'arena this' ...") even though, in Claude Code, a skill with `disable-model-invocation: true` has its "Description not in context" (https://code.claude.com/docs/en/skills). *Inferred*: the descriptions serve the `/` menu and Cursor, not model routing.

### Subagents

- Router runs in the chat that received it. It "owns the playbook, the todo list, and every subagent spawn" (`pstack/README.md`).
- A custom agent `poteto-agent` (`pstack/agents/poteto-agent.md`) must read `poteto-mode/SKILL.md` in full before work: "Substituting `generalPurpose` skips that read and drifts."
- **Fresh subagents by default.** A retry or fix round goes to a new agent with the original brief, later directives and the prior report. Resume only when state is costly to move (checkout, uncommitted changes, running process) (`poteto-mode/SKILL.md` line 100).
- "You own every subagent's work. Review the diff and write your own summary. A second opinion is the same prompt on another vendor's model." (line 98).
- Model per role, written once by `/setup-pstack` to an always-applied rule (`~/.cursor/rules/pstack-models.mdc`, or `~/.claude/rules/pstack-models.md`); every skill reads its role line and falls back to a default (`pstack/README.md` "make it yours").

### Review loops

- `interrogate`: adversaries on different models try to break the diff. "A finding survives only when a run proves it." Style, naming and "risks that need a future change to matter" are excluded. Lead re-runs each finding's command before keeping it. Reports only, never fixes.
- `arena`: N candidates, a rubric (3-6 criteria) the candidates never see, one judge from a vendor other than the parent's, pick a base, hand-graft "one or two things" from losers. Wild divergence means the frame was under-specified: reframe, don't average.
- `show-me-your-work`: before hand-back, a subagent on another vendor reads the log and transcript and the reply ends with an "Attention" section naming the reviewer model.
- Verdict vocabulary everywhere: VERIFIED / NOT VERIFIED / INCONCLUSIVE, "Inconclusive is not a pass" (`skills/figure-it-out/SKILL.md` Phase C).

### State and doc files

Ephemeral by default: todo list, worktrees, `decisions.tsv` (uncommitted unless a reviewer needs it), `/tmp` resume note, `~/.*/rules/pstack-models.*`. Nothing like a project roadmap or spec store. `/correct` writes a rule table into the agent instruction file. Docs-to-tree drift is enforced by CI: `scripts/check-pstack-docs.mjs` (repo root) checks README tables, counts written as words ("twenty-two playbooks"), slash commands, links and model names against the skills folder, and exits 1 on drift.

---

## 4. Distinctive ideas, against hanseo-skills

Compared with `README.md`, `CONTEXT.md`, `.agents/invocation.md`, and `skills/*/SKILL.md` at this branch. Fit is *inferred*.

| # | Idea | Source | Present in hanseo-skills? | Fit |
|---|---|---|---|---|
| 1 | **Run-proven review**: a review finding survives only with a command and its output; lead re-runs it; style and speculative risk dropped | `skills/interrogate/SKILL.md` | No. `stint` caps review at two passes and notes judgement-call findings never converge, but does not require proof per finding | **Good.** Matches evidence-based values and gives `stint`/`han-implement-spec` a reason to drop findings instead of a pass cap alone |
| 2 | **Blinded skill evals**: candidate prompt looks organic, no "eval/test/judge" words in paths, rubric hidden, judge sees sanitized labels, chain-following graded from transcripts (files actually opened), never self-report | `skills/poteto-mode/playbooks/eval.md` | Partly. CLAUDE.md requires RED/GREEN subagent baselines and `.agents/evals/` holds cases. No blinding or transcript grading rule | **Good.** Tightens the existing verification rule without new files |
| 3 | **Ask only what no run can settle**: an empirical fork gets a throwaway prototype; the operator is asked only product or preference calls | `poteto-mode/SKILL.md` line 21, `playbooks/multi-phase-plan.md` step 2, `skills/sharpen/SKILL.md` | Partly. `han-grill` probes code structure, but asks the user on empirical forks too | **Partial.** Fits han-grill's "grounded recommendations". Conflicts with pstack's wider "never block on the human", which would gut the interview skills |
| 4 | **Rule propagation to subagents**: first line of every kickoff is `Read <path>/worker.md`, because rules in user-invoked skills reach no other agent; the worker file holds the bar ("the bar in your kickoff is the whole bar"), no-question rule, PR format | `skills/poteto-mode/worker.md`, SKILL.md line 20 | No. `han-implement-spec` spawns agents and restates its rules in briefs (*inferred* from its outline, not checked line by line) | **Good** if han-implement-spec briefs drift. One small file, single source of truth for delegate rules |
| 5 | **Fix-level ladder for repeated mistakes**: architecture, then types, then a lint whose error names the fix, then a test, docs last; prove each check fails on a real past mistake; keep a table pairing each rule with its enforcer | `skills/correct/SKILL.md` | Partly. `groundwork` and `project-init` guardrails carry a `Check:` field (`CONTEXT.md` "guardrail"). No ladder, no "prove on a past mistake" | **Good.** Could sharpen how guardrail `Check:` is chosen. Not a new skill |
| 6 | **Transcript-mined skill edits**: read the session, keep a learning only if it holds in six months, changes a future decision, and routes to a skill used in the session; Accepted / Rejected / Backlog; approval before any edit; enforceable items go to backlog, not prose | `skills/reflect/SKILL.md` (17 lines) | No | **Good, low cost.** Fits evidence-based and short. Would be a new skill: needs README row and RED/GREEN |
| 7 | **Append-only decision log with evidence pointers**, audited against the transcript at the end, then reviewed by another vendor's model | `skills/show-me-your-work/SKILL.md` | Partly. Docket keeps design decisions (D ids). No per-run log | **Weak.** Overlaps docket decisions and stint's fixed point. Useful only for long unattended runs, which hanseo does not target |
| 8 | **Arena**: N attempts, hidden rubric, cross-vendor judge, pick base, graft by hand; divergence = bad frame | `skills/arena/SKILL.md` | No | **Weak.** Costly multi-model fan-out; conflicts with han-implement-spec's "strong models only where needed" |
| 9 | **Principles as one-rule leaf skills** with a router index phrased by trigger, and a rule to cite in the reply only principles whose leaf was read this session | `poteto-mode/SKILL.md` lines 15, 31-72 | No principle layer | **Conflict risk.** The index restates each leaf (two sources), kept honest by a CI script hanseo lacks. The "cite only what you read" rule is cheap and portable on its own |
| 10 | **Docs drift as a CI check**: script compares README tables, counts and slash names to the skills folder | `scripts/check-pstack-docs.mjs` | No. hanseo's anti-drift rule ("adding a skill → update root README") is prose only | **Possible.** pstack's own `encode-lessons-in-structure` says the same. Speculative until the README actually drifts (CLAUDE.md section 3) |
| 11 | **Pause and pickup pair**: pause = finish atomic step, `wip:` commit, resume note; pickup = "the prior trail is authoritative input", don't re-derive, but verify inherited claims against the goal on the real artifact | `playbooks/pause-safely.md`, `session-pickup.md` | Partly: docket and groundwork resume from their docs | **Partial.** The "trust the trail, verify the claim" split is portable wording for docket resume |
| 12 | **Fresh subagent per retry** with original brief + prior report | `poteto-mode/SKILL.md` line 100 | Not stated | **Good**, one line in han-implement-spec |
| 13 | **Throughput checkpoint**: four fixed todo items before fan-out, `n/a: <reason>` instead of dropping one | `playbooks/feature.md` step 3 | han-implement-spec chooses sequential or parallel per frontier (`CONTEXT.md`) | **Partial.** The "keep the item, write n/a with reason" pattern prevents silent skips |
| 14 | **No planning skills; best spec is code** | `pstack/README.md` | Opposite: docket, han-to-spec, han-to-tickets | **Conflict.** hanseo's premise is that multi-session design needs written specs |
| 15 | Model roles persisted in a config rule written by a setup skill | `skills/setup-pstack/SKILL.md` | hanseo names model tiers and lets the harness map them (`CONTEXT.md` "model tier") | **Conflict / skip.** Config for values hanseo deliberately leaves to the harness |

---

## 5. Composition: how one pstack skill invokes another

**Mechanism: name in bold prose, resolved by reading the file at a relative path. No frontmatter field, no Skill-tool call.**

1. **Reference form.** Skills name others as "the **arena** skill", "run **Opening a PR**", "per the **prove-it-works** principle skill" (e.g. `skills/interrogate/SKILL.md` line 17, `skills/figure-it-out/SKILL.md`). Principle leaves follow a naming rule: "**principle-<title>** skill, with the title in kebab case" (`poteto-mode/SKILL.md` line 33).
2. **Resolution.** "Bundled skills live at `../<name>/SKILL.md` from this file" (`poteto-mode/SKILL.md` line 11). Playbooks are plain files, not skills: "open its steps at `playbooks/<title>.md`".
3. **Why reading, not invoking.** Nearly every skill has `disable-model-invocation: true`, so the model cannot call it. The Claude Code adapter says so: "Run another pstack skill (`/name` or 'the **name** skill') → `Read` `<pstack>/skills/<name>/SKILL.md` and follow it. The `Skill` tool refuses most pstack skills." (`pstack/docs/harness/claude-code.md`, Tools table). Claude Code's docs confirm the block: with `disable-model-invocation: true`, "Claude can't invoke the skill on its own" and if Claude tries without `/name` in the user's message, "Claude Code blocks the call" (https://code.claude.com/docs/en/skills).
4. **Partial reuse.** A skill can run named phases of another with stated changes: interrogate runs "Phase A steps 3 and 4 and Phase B of the **arena** skill, with four changes" and "Skip arena Phases C, D, and E".
5. **Ownership of formats.** "Other skills route their audit trail here instead of inventing one. Reference it by name and let it own the format. Don't restate the columns." (`skills/show-me-your-work/SKILL.md` "Composing this skill").
6. **Across subagents.** The rules do not travel with a spawn, so the kickoff's first line points at `worker.md`, and `poteto-agent` locates `poteto-mode/SKILL.md` by absolute path, falling back to searching `~/.cursor/plugins/` and picking the highest `plugin.json` version (`pstack/agents/poteto-agent.md`).
7. **Routed skills set their own subagent type.** "Routed workflow skills (`interrogate`, `swarm`) set their own `subagent_type`. Respect it." (`poteto-mode/SKILL.md` line 86).
8. The port adds a third route: a SessionStart hook injects routing text every session (`michael-denyer/pstack-claude`, `plugins/pstack/hooks/hooks.json`). Upstream has no hook.

### Bearing on hanseo-skills

- `.agents/invocation.md` says skills reference each other "by slash command name in prose, not by file path". pstack does the reverse because its skills are bundled in one plugin, so `../<name>/` always resolves. hanseo skills install separately (`npx skills add ... --skill <name>`), so a relative path may not resolve. *Inferred.*
- `skills/han-grill-with-docs/SKILL.md` says "Call the Skill tool twice, for `han-grill` and `domain-modeling`". That works for `han-grill`, which has no `disable-model-invocation`. Per the Claude Code docs above, the same instruction would be blocked for any hanseo skill marked user-invoked (docket, stint, han-to-spec, and others) unless the user typed its slash command in the same message. `domain-modeling`'s frontmatter was not checked: **inconclusive**.
- So a hanseo skill that must run a user-invoked sibling has three options: tell the user to type it (current closings that suggest a next step), read the sibling's SKILL.md by path when installed together, or make the sibling model-invoked. pstack chose the second.

---

## Conflicts and gaps

- Author's own announcement page could not be read (redirect). Design intent above comes from the README, guide and skill files only.
- pstack's skill counts in third-party listings (e.g. "57 skills", "21 principles") differ from the tree (58 skills, 24 principles). The tree is used here.
- pstack targets Cursor first. Claude Code behaviour is through a name-mapping file the agent reads at runtime, which the pstack-claude port calls a token and accuracy cost (its README, "Built for Claude Code"). Neither side measured it.
- No pstack file reports a measurement of its own effect. The "10,000 agent runs" title is a usage count, not an outcome.
