# hanseo-skills

Agent skills that fill gaps in agentic workflow tools. If you use brainstorming + spec-writing with Claude Code or superpowers, these handle three problems that workflow leaves open: durability across sessions, coherence in multi-session designs, and scoping the run that finally builds the thing.

## Why these skills exist

**The problem with multi-session agent work:**

You ask an agent to design messaging in one brainstorming session. It produces something that reads like a finished design but isn't — edge cases go unnamed, architecture choices get made by accident, placeholder decisions ("show an error") stand in for real ones. The gaps are hard to spot because the writing is confident.

The fix is obvious: split it. One session on persistence, another on sync, another on the UI. Each one is small enough to actually think about — so the spec is thorough.

But splitting introduces its own failure: **session 4 contradicts session 1.** You decided in the UI session that messages appear instantly and reconcile later. But the sync model you picked two sessions after that can't support it. Nothing catches it because each session ended in its own file and nobody re-reads the old ones. You find out at implementation time.

**What hanseo-skills does:**

- **charter** captures architectural decisions *before* the first brainstorming session, so later sessions inherit them instead of re-deriving them.
- **docket** explores a feature and creates one evolving roadmap. Reference it during slice discussions, then invoke your spec skill separately. Carried instructions let later sessions update decisions, slices and completion evidence without invoking Docket again.
- **stint** builds once the designing is done, in sittings sized by how much codebase a run has actually read rather than by ticket count. It pins the point review measures against before writing any code, and stops reviewing after two passes instead of looping toward a "clean" that judgement-call findings never reach.

Together, they make multi-session agent work sustainable: decisions are written down once, inherited durably, contradictions surface early, and the build runs stay small enough to review.

One skill sits upstream of all three: **product-discovery** decides whether a product is worth building at all, and how much proof that call needs, before any design session starts.

## Skills

| Name | Description | Type |
|------|-------------|------|
| [charter](skills/charter/README.md) | Produce a project's decision layer (architecture.md, ADRs, CLAUDE.md) | user-invoked |
| [docket](skills/docket/README.md) | Create an evolving feature roadmap for user-invoked slice specs | user-invoked |
| [stint](skills/stint/README.md) | Build one sitting's worth of tickets per run, with review's fixed point pinned up front and a hard stop after two review passes | user-invoked |
| [product-discovery](skills/product-discovery/README.md) | Decide what independent product to make, or whether to keep going with one, ending in a build / validate / narrow / kill call sized to the stakes | model-invoked |

### Adapted skills

Independent adaptations of Matt Pocock's workflows, with selected design
practices from Superpowers brainstorming. Install any of them separately;
none requires Docket or the other adaptations.

| Name | Description | Type |
|------|-------------|------|
| [han-grill](skills/han-grill/README.md) | Stress-test decisions with meaningful options, explicit tradeoffs and grounded recommendations | user-invoked |
| [han-to-spec](skills/han-to-spec/README.md) | Write a spec to the configured tracker or usual spec folder, with outcome stories, detailed UX criteria and source coverage | user-invoked |
| [han-grill-with-docs](skills/han-grill-with-docs/README.md) | Run han-grill while recording settled terms and decisions in the project's existing glossary and decision record | user-invoked |
| [han-to-tickets](skills/han-to-tickets/README.md) | Publish approved vertical tickets to the configured tracker with scope, complexity, blast radius and an implementer briefing in their bodies | user-invoked |
| [han-implement-spec](skills/han-implement-spec/README.md) | Implement a spec's ticket graph on one integration branch, with agents, worktrees, strong models and full test runs only where the work needs them | user-invoked |

## Install

Browse and install skills from this collection:

```
npx skills add Hanseooo/hanseo-skills
```

See each skill's README for its specific install command and usage.
