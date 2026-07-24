<!--
Template notes (do not copy into output):
- Skip this artifact entirely at the prototype tier.
- If an architecture doc already exists, amend it — never generate a second one.
- Every row in "Abstraction layers" MUST have a volatility reason. No reason = the
  layer moves to "Deliberately not abstracted" or gets cut.
-->
# <Project> — Architecture

*<date>. Companion to the ADRs and the operating contract. Folders below are a map,
not a scaffold — structure materializes when the first module is built.*

## System shape
<2–5 sentences: the modules, the direction data flows, where state lives.>

## Module boundaries
| Module | Responsibility | Talks to | Contract between them |
|---|---|---|---|
| <name> | <one concern> | <modules> | <typed schema / interface, named> |

## Abstraction layers
A layer exists because something behind it is expected to change.

| Layer | Hides | Exposes | Volatility reason |
|---|---|---|---|
| <name> | <vendor SDKs, auth, retries…> | <domain-shaped operations> | <what will change, per whom> |

## Deliberately NOT abstracted
- <point> — <why no change is expected here>

## Folder map
One home per artifact type. New file types get a home here before they get a folder.

| Path | Holds |
|---|---|
| <path> | <artifact type> |
