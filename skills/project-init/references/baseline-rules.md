# Baseline rules block

Add a `## Baseline Rules` block to `AGENTS.md` when the user says the repo is for
a team, or confirms it in the interview. Teammates' agents don't share one global
configuration, so this block carries the general rules the rest of the file omits.

Keep it to about a dozen bullets, a distillation and never a verbatim copy of
anyone's global file. Personal preferences, such as report format or commit
trailers, stay out. Head it with one line: where the reading agent's own global
rules are stricter, the stricter rule wins. Drop any bullet the repo's other
sections already cover, and any that can't apply, such as UI in a repo with none.

- **Push back.** A stated cause or fix is a hypothesis. Challenge a wrong one and
  say why. Change position on new evidence or a better argument, not on repetition.
- **Check before building.** A fact about the code or system is checked first.
  Stop at the first rung that settles it: run it or read the code; installed
  source or `--help`; official docs for the version in use; ask if only a person
  knows or a wrong guess is costly; otherwise label it inferred and proceed.
- **Debug on evidence.** A bug gets a command that goes red on it and two or more
  candidate causes before a fix. Search the whole repo before calling code dead.
- **Simplicity.** Does it need to exist → does stdlib or the platform cover it →
  does an installed dependency cover it → minimum code that works. A plan goes
  through this before any code. No abstraction with one implementation.
- **Simplicity stops at correctness.** Input validation at trust boundaries,
  error handling that prevents data loss, and security measures count as
  requested. A stated simplicity rationale never downgrades a review finding.
- **Follow the pattern in place.** Reuse before writing. Extract on the third
  occurrence. A second pattern needs approval to migrate the first or keep both,
  unless the project marks the first legacy. A changed fact changes everywhere it
  is stated, in the same change.
- **UI keeps its states.** Loading, empty, error and disabled states; a way out of
  every view; confirmation or undo before a destructive action; keyboard
  navigation, focus order and accessible names. Reuse the platform control.
- **Surgical changes.** Every changed line traces to the request. Match existing
  style. Remove only the orphans your own change created. A gain for users,
  developers or agents may not hide a cost to another: report it.
- **Approval gates.** New dependency, service or datastore; schema or API
  contract change; auth, billing or infra; more than three modules; hard to
  reverse.
- **The code wins.** A plan or doc the codebase contradicts is not a spec to
  satisfy. Deviate, name what you found, and flag the drift.
- **Verify before done.** Run the check and show its output. Done is observed on
  the real artifact; tests and builds are proxies. A subagent's report is a claim
  until its diff or output is checked.
- **Tests prove behavior.** Each new test fails for the intended reason first.
  Expected values are hand-worked, never computed by the code under test. Test
  through the public interface; mock only third-party calls, the clock and the
  network. A failing test's expectation changes only when the test is wrong.
- **Security.** Never read `.env` or secret files. Never echo credentials. Stop
  at the login wall and hand it back.
