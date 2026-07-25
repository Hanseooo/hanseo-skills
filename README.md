# hanseo-skills

Agent skills that fill gaps in agentic workflow tools. If you use brainstorming + spec-writing with Claude Code or superpowers, these handle two problems that workflow leaves open: durability across sessions and coherence in multi-session designs.

## Why these skills exist

**The problem with multi-session agent work:**

You ask an agent to design messaging in one brainstorming session. It produces something that reads like a finished design but isn't — edge cases go unnamed, architecture choices get made by accident, placeholder decisions ("show an error") stand in for real ones. The gaps are hard to spot because the writing is confident.

The fix is obvious: split it. One session on persistence, another on sync, another on the UI. Each one is small enough to actually think about — so the spec is thorough.

But splitting introduces its own failure: **session 4 contradicts session 1.** You decided in the UI session that messages appear instantly and reconcile later. But the sync model you picked two sessions after that can't support it. Nothing catches it because each session ended in its own file and nobody re-reads the old ones. You find out at implementation time.

**What hanseo-skills does:**

- **charter** captures architectural decisions *before* the first brainstorming session, so later sessions inherit them instead of re-deriving them.
- **docket** plans and tracks multi-session feature designs. Each session gets a narrow scope, an anti-scope, and a list of decisions locked in from prior sessions. Sessions can't contradict each other because contradictions happen in the docket file, while it's still cheap to fix.

Together, they make multi-session agent work sustainable: decisions are written down once, inherited durably, and contradictions surface early.

## Skills

| Name | Description | Type |
|------|-------------|------|
| [charter](skills/charter/README.md) | Produce a project's decision layer (architecture.md, ADRs, CLAUDE.md) | user-invoked |
| [docket](skills/docket/README.md) | Split one large feature into ordered, narrowly-scoped design sessions tracked in a durable file | user-invoked |

## Install

Browse and install skills from this collection:

```
npx skills add Hanseooo/hanseo-skills
```

See each skill's README for its specific install command and usage.
