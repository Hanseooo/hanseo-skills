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

## Keep the two harness flags in step

Claude Code reads `disable-model-invocation: true` in `SKILL.md`; Codex reads `policy.allow_implicit_invocation: false` in `agents/openai.yaml`. A user-invoked skill sets both. A model-invoked skill sets neither and ships no `openai.yaml` policy. A leftover `false` policy hides it from the Codex model, and then no other skill can reach it there.

## Composition

A skill may load a model-invoked skill; it can never reach a user-invoked one, since both harnesses hide those from the model. Name a user-invoked next step for the user to run instead: "run `/han-to-spec`".

Skills reference each other by slash command name in prose, not by file path, because each skill installs separately and a relative path may not resolve. Example: "run `/charter` before building" not "see `skills/charter/SKILL.md`". A bare slash name is enough for the agent to load a model-invoked skill (`.agents/evals/review-triage.md`, C1).
