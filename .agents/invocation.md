# Skill Invocation Classification

## User-invoked

A skill is **user-invoked** when it requires a human to type its slash command to activate. The agent never triggers it automatically.

Use this classification for:
- Orchestration skills that kick off a full workflow
- One-shot flows where human intent must be explicit

## Model-invoked

A skill is **model-invoked** when the agent may activate it based on task context — it can also be triggered by explicit user request, but the agent doesn't wait to be asked.

Use this classification for:
- Reusable discipline loops (testing, debugging, reviewing)
- Skills the agent can meaningfully choose to apply without being told

## Decision test

Could an agent meaningfully choose to use this skill without a human naming it? If yes → model-invoked. If it only makes sense when a human explicitly initiates it → user-invoked.

## Reference convention

Skills reference each other by slash command name in prose, not by file path. Example: "run `/charter` before building" not "see `skills/charter/SKILL.md`".
