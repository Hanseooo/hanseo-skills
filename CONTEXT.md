# Domain Vocabulary

- **skill** — a packaged set of agent instructions, living in `skills/<name>/`, invokable by name
- **SKILL.md** — the machine-readable instructions file at the root of a skill folder; what agents load and follow
- **user-invoked** — a skill only reachable when a human types its slash command; never triggered automatically by the agent
- **model-invoked** — a skill the agent may reach for automatically when the task fits, as well as by explicit user request
- **operating contract** — the `CLAUDE.md` / `AGENTS.md` files that define how agents must behave when working on this repo
- **docket** — a durable file listing the design sessions a large feature needs, their order, and the decisions already settled; lives in the user's project, not this repo
- **session** — one narrowly-scoped design conversation covering a single cluster of the docket's open questions, ending in an approved spec
- **binding constraint** — a short decision statement extracted from a finished session and confirmed by the user; later sessions treat it as given, not open
- **engine** — the one skill in a session that writes the spec file; without it a session cannot reach `DONE`
- **hardener** — a skill that sharpens a session but writes no spec, so it never substitutes for the engine
- **parked finding** — something real a session turned up that belongs to no session in the docket; recorded in one line and deliberately not acted on
