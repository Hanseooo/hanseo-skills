# hanseo-skills

Agent skills that fill gaps in agentic workflow tools. If you use brainstorming + spec-writing with Claude Code or superpowers, these handle three problems that workflow leaves open: durability across sessions, coherence in multi-session designs, and scoping the run that finally builds the thing.

## Why these skills exist

**The problem with multi-session agent work:**

You ask an agent to design messaging in one brainstorming session. It produces something that reads like a finished design but isn't — edge cases go unnamed, architecture choices get made by accident, placeholder decisions ("show an error") stand in for real ones. The gaps are hard to spot because the writing is confident.

The fix is obvious: split it. One session on persistence, another on sync, another on the UI. Each one is small enough to actually think about — so the spec is thorough.

But splitting introduces its own failure: **session 4 contradicts session 1.** You decided in the UI session that messages appear instantly and reconcile later. But the sync model you picked two sessions after that can't support it. Nothing catches it because each session ended in its own file and nobody re-reads the old ones. You find out at implementation time.

**What hanseo-skills does:**

- **charter** captures architectural decisions *before* the first brainstorming session, so later sessions inherit them instead of re-deriving them.
- **docket** maps one large feature's design in a single file: the spec it ends in, the open questions in order, and every decision recorded with what it binds. Each session settles one question against the decisions before it, so contradictions surface in the docket while they're still cheap to fix, and the design ends as one spec instead of five that overlap.
- **stint** builds once the designing is done, in sittings sized by how much codebase a run has actually read rather than by ticket count. It pins the point review measures against before writing any code, and stops reviewing after two passes instead of looping toward a "clean" that judgement-call findings never reach.

Together, they make multi-session agent work sustainable: decisions are written down once, inherited durably, contradictions surface early, and the build runs stay small enough to review.

One skill sits upstream of all three: **product-discovery** decides whether a product is worth building at all, and how much proof that call needs, before any design session starts.

## Skills

| Name | Description | Type |
|------|-------------|------|
| [charter](skills/charter/README.md) | Produce a project's decision layer (architecture.md, ADRs, CLAUDE.md) | user-invoked |
| [docket](skills/docket/README.md) | Design one large feature across several sessions, one question at a time in a durable file, ending in a single spec | user-invoked |
| [stint](skills/stint/README.md) | Build one sitting's worth of tickets per run, with review's fixed point pinned up front and a hard stop after two review passes | user-invoked |
| [product-discovery](skills/product-discovery/README.md) | Decide what independent product to make, or whether to keep going with one, ending in a build / validate / narrow / kill call sized to the stakes | model-invoked |

## Install

Browse and install skills from this collection:

```
npx skills add Hanseooo/hanseo-skills
```

See each skill's README for its specific install command and usage.
