# Audit an existing instruction file

Check every line against the evidence. A finding names the line and what shows
it is wrong. Each one lands in the proposed diff, or in the report when another
owner's section holds it.

| Look for | Evidence |
|---|---|
| A documented command with no source | The manifest, task runner and CI |
| A test, lint, typecheck or e2e script nothing documents | The script |
| A run command or test login source the file omits | The README and scripts |
| A pointer to a path that doesn't exist | The file listing |
| A rule any agent would follow anyway, or one general to all projects | The line itself |
| History or status: renames, dated entries, what was built | The line itself |
| Config cached in prose: stack lists, versions, folder trees | The file holding it |
| A rule repeated in another doc | Both docs. It stays with its owner. |
| Material only some tasks need, sitting inline | A `Read when` pointer can replace it |
| A `CLAUDE.md`, `.claude/CLAUDE.md` or `CLAUDE.local.md` without `@AGENTS.md` | `AGENTS.md` does not load in Claude Code |
| Over 200 lines, or near Codex's 32 KiB combined limit | The file size |

An invariant a test, linter or type could enforce goes in the report as a
suggestion. The prose stays until a check exists.

Keep what the repo already does: its section names, doc paths and numbering. A
patch changes findings only.
