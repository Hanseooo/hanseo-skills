---
name: han-grill-with-docs
description: Run han-grill while recording settled terms and decisions in the project's existing glossary and decision record.
disable-model-invocation: true
---

# han-grill-with-docs

Call the Skill tool twice, for `han-grill` and `domain-modeling`, and follow
both. If either is missing, name it with its install command and stop:

- `npx skills add Hanseooo/hanseo-skills --skill han-grill`
- `npx skills add mattpocock/skills --skill domain-modeling`

These rules take precedence over both skills:

- **Write where the project already writes.** Before the first round, find the
  existing glossary (`CONTEXT.md`, `GLOSSARY.md`, `GLOSSARY-MAP.md`, or a
  vocabulary file the agent docs point to) and decision record (`docs/adr/`, a
  decisions table, or a log the docs point to). Add to them in their own format
  and numbering. Create `GLOSSARY.md` or `docs/adr/` only when no such file
  exists.
- **Docs are the only writes.** Glossary and decision-record edits happen during
  the interview; spec, tickets, code and commits stay separate requests, as
  `han-grill` sets out. The closing recap lists each doc change by path and
  entry.
