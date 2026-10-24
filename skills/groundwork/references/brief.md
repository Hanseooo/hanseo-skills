# Project brief

The destination every other doc and session starts from. Every session reads it
first, so it stays short enough to read in a minute. It changes rarely: a scope
change is the user's decision.

Default path: `docs/brief.md`. Title it by kind ("Product brief", "Library
brief") when that helps.

## Sections

Always:

- **Purpose**: one paragraph on what this is and why it exists.
- **Serves**: who uses it and what they do with it. For a library, that is the
  developers calling it; for client work, the client and their users.
- **Done looks like**: observable outcomes of the first complete version.
- **In scope / Out of scope**: out of scope is as specific as in scope.
- **Project rules**: rules that bind several milestones, each with a stable
  `P-01` ID that specs cite. A rule only one slice needs belongs in that
  slice's spec.
- **Open questions**: what is unresolved, and what each blocks.

Include when the project has one:

- **Constraints**: a deadline, budget, platform, licence or compliance limit.
- **Success measures**: a metric someone will actually check.
- **Sign-off**: client work, naming who approves what.
- **Public surface**: libraries, APIs and CLIs, covering what is supported and
  the compatibility promise.
- **Replaces**: an existing tool or process, and what must carry over.

Technology choices belong to architecture, milestone order to the roadmap and
feature behavior to specs. The brief links to them once they exist.
