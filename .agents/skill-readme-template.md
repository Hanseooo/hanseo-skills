# Skill README Template

Every skill's `README.md` follows this shape. Copy the block below, fill it in,
delete every `<!-- -->` comment and any section marked optional that you didn't
need.

The README is for a human deciding whether to install. `SKILL.md` is for the
agent executing. Don't duplicate `SKILL.md`'s rules here — say what the skill is
for and what it costs, and let the agent read the rules.

---

```markdown
# <skill-name>

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

<!-- Two or three lines. What it's for, in the reader's terms, not the skill's.
     Name the thing it produces or the decision it makes. No feature list. -->

## Where this sits

<!-- What this assumes you already have, and what it hands off to. Name the
     neighbouring skills by slash command. If the skill is derived from or
     inspired by someone else's, say so here plainly — credit costs nothing and
     an unattributed near-copy reads worse than an acknowledged one.
     If it degrades without an external skill, say which and how. -->

## The problem

<!-- The failure this exists to prevent, described concretely enough that a
     reader recognises it from their own work. Prefer an observed failure over a
     hypothetical one — if you ran the RED phase, this is where its findings go.
     Don't sell; describe. -->

## How <skill-name> solves it

<!-- The two or three load-bearing ideas, each with the reason it works. This is
     the section that earns the install. Keep it to mechanisms, not steps —
     a numbered process belongs in SKILL.md. -->

## When not to use it

<!-- Required. The cases where the reader should close the tab and use something
     else, named. A skill with no stated limits reads as a skill nobody tested.
     Include the cheaper alternative for each case. -->

## Install

<!-- Required by the operating contract. Keep the collection-level command and
     the standalone-clone fallback; adjust only the skill folder name. -->

Via the [skills.sh](https://www.skills.sh) CLI (works for Claude Code and other
supported agents):

```
npx skills add Hanseooo/hanseo-skills
```

Or clone the collection and reference this skill's folder directly:

```
git clone https://github.com/Hanseooo/hanseo-skills
```

- **Claude Code:** clone into `~/.claude/skills/<skill-name>` (use
  `skills/<skill-name>/` as the skill root).
- **Codex, opencode, Antigravity CLI, or others:** clone anywhere and load
  `skills/<skill-name>/SKILL.md` per that tool's own custom-instructions/skill
  mechanism, or paste `SKILL.md`'s contents into the session.

Update later with `git pull` (or re-run `npx skills add`).

<!-- If the skill calls external skills, list them here and say what you lose
     without them. -->

## Use

<!-- What the reader types, and what comes back. One or two real invocations,
     copy-pasteable. Then one short paragraph on what happens between the
     invocation and the result. Not a rule dump. -->

## FAQ

<!-- OPTIONAL. Include when the skill sits near something the reader already
     knows — a skill it derives from, a built-in it resembles, or a neighbour it
     is easy to confuse with. Skip it entirely when nothing needs disambiguating;
     an FAQ of invented questions is worse than no FAQ.

     Use real questions someone actually asked, phrased as they asked them.
     Answer in a way that can lose: if the other tool is the better choice in
     some case, say which case. A comparison where you win every row is a
     comparison nobody believes.

     Question types worth covering:
       - "How is this different from <the skill it derives from>?"
       - "When should I use <that one> instead?"
       - "Do I still need <neighbour skill>?"
       - "Why does it refuse to <thing readers expect it to do>?"
       - "What does it cost?" — tokens, sessions, added ceremony -->

### How is this different from `<other>`?

<!-- A short table works well when the difference is per-behaviour. Prose is
     better when the difference is one idea. Don't do both. -->

### When should I use `<other>` instead?

<!-- Name the cases where the other tool wins, without hedging them into
     nothing. -->

## What it deliberately is not

<!-- The adjacent jobs people will assume it does. One line each. This is a
     scope fence, not modesty. -->

## License

[MIT](LICENSE)
```

---

## Notes

- **Ship a `LICENSE` in the skill folder.** The badge links to it relatively, so
  a skill cloned on its own has a dead link without it. Copy the repo root's.
- **Section order is deliberate.** Problem before solution, limits before
  install, install before use. A reader who bails at "When not to use it" was
  never going to be a happy user.
- **Every section is required except `## FAQ`.** If a required section has
  nothing to say, that is a signal about the skill, not about the template.
- **Don't restate `SKILL.md`.** The two files drift, and when they do the README
  is the one that lies — nobody re-reads it after the rules change.
