# Domain Vocabulary

- **skill** — a packaged set of agent instructions, living in `skills/<name>/`, invokable by name
- **SKILL.md** — the machine-readable instructions file at the root of a skill folder; what agents load and follow
- **user-invoked** — a skill only reachable when a human types its slash command; never triggered automatically by the agent
- **model-invoked** — a skill the agent may reach for automatically when the task fits, as well as by explicit user request
- **operating contract** — the `CLAUDE.md` / `AGENTS.md` files that define how agents must behave when working on this repo
