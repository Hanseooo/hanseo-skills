# Baseline rules block

Add a short `## Baseline Rules` block to `AGENTS.md` only when one of these
holds:

- The repo shows more than one agent tool in use (a `.cursor/` directory,
  Copilot instructions, `GEMINI.md`), so no single global config reaches every
  agent that touches it.
- The repo is worked by agents whose global configuration the user does not
  control: open source, a client handoff, a team on mixed tooling.
- The user asks for it.

Keep it to about a dozen bullets, a distillation and never a verbatim copy of
anyone's global file. Head it with one line: where the reading agent's own
global rules are stricter, the stricter rule wins. Drop any bullet the repo's
other sections already cover.

- **Think before coding.** State assumptions. Search the whole repo before
  calling anything dead. Read the code you are changing rather than inferring it.
- **Simplicity.** Does it need to exist → does stdlib or the platform cover it →
  does an installed dependency cover it → minimum code that works. Fewest moving
  parts, not fewest characters. No abstraction with one implementation.
- **Simplicity stops at correctness.** Input validation at trust boundaries,
  error handling that prevents data loss, and security measures count as
  requested. A stated simplicity rationale never downgrades a review finding.
- **Reuse before writing.** Search for an existing helper before adding one.
  Extract on the third occurrence, not the first. One source of truth per fact:
  a fact that changes changes everywhere it is stated, in the same change.
- **UI is exempt from cutting.** Loading, empty, error and disabled states; a way
  out of every view; confirmation or undo before a destructive action; keyboard
  navigation, focus order and accessible names. Reuse the platform control.
- **Surgical changes.** Every changed line traces to the request. Match existing
  style. Remove only the orphans your own change created.
- **Approval gates.** New dependency, service or datastore; schema or API
  contract change; auth, billing or infra; more than three modules; hard to
  reverse.
- **The code wins.** A plan or doc the codebase contradicts is not a spec to
  satisfy. Deviate, name what you found, and flag the drift.
- **Evidence over inference.** A fact about the code or system is checked before
  anything is built on it, or labeled inferred. A bug gets a command that goes
  red on it and two or more candidate causes before a fix.
- **Verify before done.** Run the check and show its output before claiming
  complete. Done is observed on the real artifact; tests and builds are proxies.
  A subagent's report is a claim until its diff or output is checked.
- **Tests prove behavior.** Each new test fails for the intended reason first.
  Expected values are hand-worked, never computed by the code under test. Test
  through the public interface; mock only third-party calls, the clock and the
  network. A failing test's expectation changes only when the test is wrong.
- **Security.** Never read `.env` or secret files. Never echo credentials. Stop
  at the login wall and hand it back.
- **Commits.** No co-author or generated-by trailers. Several harnesses add these
  automatically, so this one has to be stated to hold.
