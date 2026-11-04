# What makes agent docs and architecture help coding agents and limit drift

Retrieved 2026-11-04. Input to an audit of `skills/groundwork/`. It makes no recommendations.

## How to read this file

Evidence tags:

- **[measured]**: a study or dataset reports a number. The tag says nothing about strength, so each entry also gives design and status.
- **[vendor guidance]**: the maker of a tool or owner of a format says so. Covers documented behaviour and advice.
- **[practitioner opinion]**: experience or argument, including self-reported numbers with no comparison group.

Other labels:

- **peer reviewed**: a venue is verified or stated by the source. **preprint**: arXiv with no venue established.
- **abstract only**: I read the abstract, not the body. Treat its numbers as the authors' summary.
- **inferred**: my reasoning, not the source's statement. **inconclusive**: the check could not settle it.

Most arXiv IDs below are 2026 preprints in a fast-moving area. Few have been replicated.

---

## Q1. Gloaguen et al., "Evaluating AGENTS.md" (arXiv 2602.11988)

Read in full: v3 HTML, https://arxiv.org/html/2602.11988 (v3 dated 29 Sep 2026). Compared with v1, https://arxiv.org/html/2602.11988v1 (12 Feb 2026). v2 is dated 23 Jun 2026.

**The numbers moved between versions, so a citation needs a version number.** All numbers in this section are v3 unless marked.

| | v1 | v3 |
|---|---|---|
| Abstract | "tend to reduce task success rates" | "does not generally improve task success rates, while increasing inference cost by over 20%" |
| Developer-written files | "an increase of 4% on average" | +2.4% on average, p=21% |
| LLM-generated files | -3% | -0.5% (SWE-bench), -2% (CTXbench) |
| Recommendation for human-written files | "including only minimal requirements" | Only what is "not already present in the README", and "rigorously evaluated before adoption" |
| Benchmark name | AGENTbench | CTXbench |

### Design [measured]

- Four setups: Claude Code with Sonnet-4.5, Codex with GPT-5.2, Codex with GPT-5.1 mini, Qwen Code with Qwen3-30b-coder.
- SWE-bench Lite: 300 tasks, 11 popular Python repos, no developer context files.
- CTXbench: 138 instances from 12 niche Python repos that have developer context files (mean 641 words, range 24-2003). Bug fixes and features.
- Each agent ran each instance once. Git history and remotes were removed. Task descriptions were standardized by LLM. Tests were LLM-generated (25 of 138 manually edited), with mean 75% coverage of modified code.
- Authors call CTXbench "slightly easier but fairer".

### Success rate, % (Table 5) [measured]

The v3 HTML labels the second benchmark's rows "Plan-Bench". They match CTXbench (*inferred* to be a leftover label). Standard errors are about 3-4.3 points, so most single-cell differences are within noise.

| Benchmark, file | Sonnet-4.5 | GPT-5.2 | GPT-5.1 mini | Qwen |
|---|---|---|---|---|
| SWE-bench, none | 59.9 | 56.6 | 47.7 | 31.1 |
| SWE-bench, LLM | 58.7 | 54.4 | 47.6 | 32.4 |
| CTXbench, none | 73.2 | 65.2 | 54.3 | 45.7 |
| CTXbench, LLM | 65.2 | 68.1 | 50.7 | 47.1 |
| CTXbench, developer | 70.3 | 68.1 | 55.8 | 53.6 |

Cochran-Mantel-Haenszel p-values:

| Comparison | p |
|---|---|
| SWE-bench, none vs LLM | 0.87 |
| CTXbench, none vs LLM | 0.37 |
| CTXbench, none vs developer | 0.21 |
| CTXbench, LLM vs developer | 0.038 (the only significant one) |

- The paper states developer files beat LLM files by about 7% on average. The table means are 61.95 vs 57.75, a 4.2 point gap, so 7% is a relative figure (*inferred*).
- Developer files helped every agent except Claude Code, where success fell from 73.2 to 70.3 (within noise).

### Steps and cost, mean per instance (Table 2) [measured]

| Benchmark, file | Sonnet-4.5 | GPT-5.2 | GPT-5.1 mini | Qwen |
|---|---|---|---|---|
| SWE-bench, none | 54.4, $1.30 | 12.5, $0.32 | 40.9, $0.18 | 29.7, $0.12 |
| SWE-bench, LLM | 57.2, $1.51 | 12.7, $0.43 | 45.2, $0.22 | 32.2, $0.13 |
| CTXbench, none | 40.7, $1.15 | 12.1, $0.38 | 40.6, $0.18 | 31.5, $0.13 |
| CTXbench, LLM | 46.5, $1.33 | 13.1, $0.57 | 46.9, $0.20 | 34.2, $0.15 |
| CTXbench, developer | 45.3, $1.30 | 13.6, $0.54 | 46.6, $0.19 | 32.8, $0.15 |

- LLM files: +2.45 steps (SWE-bench), +3.92 steps (CTXbench), cost +20% and +23%.
- Developer files: +3.34 steps, cost "at most 19%". Both increases are significant by permutation test (Table 6).

### Overviews [measured]

- Section title: "Context files do not provide effective overviews". Finding: "context files are not effective at providing a repository overview".
- Metric: steps before first touching a file in the PR patch. Not meaningfully reduced for any agent, significantly increased for GPT-5.1 mini.
- 8 of 12 developer files had an overview. LLM files flagged one 100% (Sonnet), 95% (Qwen), 99% (GPT-5.2), 36% (GPT-5.1 mini) of the time.
- Ablation (Table 7, GPT-5.2 only): removing the overview gave 62.32% vs 68.12% on CTXbench (p=.15) and 54.20% vs 54.36% on SWE-bench (p=.73). No category had a significant accuracy effect. Removing testing instructions cut cost significantly ($.4715 to $.3730, p=.023; $.3272 to $.2756, p=.0035).

### Instructions are followed, and that costs work [measured]

- `uv` used 1.6 times per instance when mentioned, under 0.01 when not. Repo-specific tools 2.5 vs under 0.05.
- More tests, grep, reads and writes with a file present.
- Reasoning tokens rose: LLM +22% (GPT-5.2) and +10% (mini) on SWE-bench, +14% and +10% on CTXbench. Developer +20% and +2%.
- Length: no clear dependency of success or cost on file length (Figure 13).

### Low-documentation setting [measured]

With all `.md`, docs and examples removed (App. B, excluding Claude Code), LLM files improve success by 2.7% on average and beat developer files. The authors' hypothesis is that context files are redundant with existing docs. This is the closest the paper comes to a low-doc repo. Relevance to greenfield is *inferred*, since the code still exists.

### Recommendation (v3, verbatim)

- "We therefore suggest omitting LLM-generated context files for the time being"
- "Human-written context files should only include instructions required for coding agents that are not already present in the README (e.g., specific conventions or non-functional requirements), and be rigorously evaluated before adoption."

### Limits

- Stated in the paper (§5): Python only, with a caveat that Python's training-data coverage may hide the value of tooling facts. Only task resolution was measured. Efficiency and security of the code are future work.
- Not in the paper as read: multi-session work, architecture drift, greenfield projects, long-term maintainability, whether a human would merge the patch, doc staleness, value to human readers.
- All tasks are one-shot issue or PR tasks on an existing repo.
- Models are older than the current generation. No evidence on whether the result holds for them.

### Peer review and venue: verified as workshops, not the main track

- Poster, ICLR 2026 workshop "Agentic AI in the Wild": https://iclr.cc/virtual/2026/10021142
- Oral, ICLR 2026 Workshop on Memory for LLM-Based Agentic Systems (MemAgents): https://iclr.cc/virtual/2026/10021235
- Authors' page, "MemAgents @ ICLR 2026, Oral & Runner-up Best Paper": https://eth-sri.github.io/publications/gloaguen2026agentsmd
- The abstract on those pages is v1-era text. v3 carries later revisions, and arXiv lists no venue comment. Whether the v2 and v3 changes were peer reviewed is **inconclusive**. The OpenReview page was not fetched.

### Other measured work on context files

| Source | Status | Design | Finding |
|---|---|---|---|
| Lulla et al., https://arxiv.org/abs/2601.20404 | preprint | Codex gpt-5.2-codex, 10 repos, 124 paired PRs | Median runtime -28.64%, median output tokens -16.58%. Correctness explicitly out of scope, 50-task sanity check only |
| Two-agent ablation, https://arxiv.org/abs/2607.27250 | preprint, partly read | Claude Code and Codex, 17 tasks, 3 Python repos, 288 runs | No correctness effect from none, always-on or selective injection, "bounded to <10-15pp". Blind full-suite runs fell 3.67, 2.44, 1.67 (directional) |
| Probe-and-Refine, https://arxiv.org/abs/2606.20512 | preprint, abstract only | SWE-bench Verified, Qwen3.5-35B | 33.0% vs 28.3% (static guidance) vs 25.5% (none), p<.001. Helps reach the right file |
| LoLBench, https://arxiv.org/abs/2609.37143 | preprint, abstract only | 2.4M LoC systems | File trees and API specs improve localization 16-22 points |
| SWE-CC, https://arxiv.org/abs/2610.06193 | preprint, abstract only | 12 repos, 823 checkable policies, 500 tasks | Agents violate 43.1% of applicable policies while functionally correct |
| Conditional phrasing, https://arxiv.org/abs/2611.04367 | preprint, abstract only | 86 tasks, 4 combinations | Conditional wording followed less than prescriptive or explanatory |

---

## Q2. Claude Code docs

Read: https://code.claude.com/docs/en/memory and https://code.claude.com/docs/en/best-practices (fetched as `.md`, CLI current at 2.1.318). All entries are **[vendor guidance]**. None reports a measurement. The hooks page was downloaded but not read, because the best-practices page covered the point needed. Hooks guide: https://code.claude.com/docs/en/hooks-guide

### What to include and exclude

Best practices:

- "Only write down what Claude can't find by reading the repo, or what it would otherwise spend time rediscovering every session."
- Test for a line: "Could it find this by reading the code? Would it actually go and look? If both are yes, leave it out."
- Reasoning: "current models are *very* good at this. They'll find your file layout, your conventions, your test framework"
- Include list: exact test, typecheck and lint commands including a single test file, "Places where the code is misleading", "Decisions that aren't in the repo", "Anything you've had to correct twice already".
- `/init` output: "the work is mostly removing lines".

Memory page:

- "Keep it to facts Claude should hold in every session: build commands, conventions, project layout, 'always do X' rules."
- Multi-step procedures or rules for part of the codebase go to a skill or path-scoped rule.
- Add a line when "Claude makes the same mistake a second time", when review catches something, or when the same correction recurs.

The two pages differ on layout. The memory page lists "project layout" as an example, while best practices lists file layout as something Claude finds itself.

### Size

- "Size: target under 200 lines per CLAUDE.md file. Longer files consume more context and reduce adherence."
- Imports "don't reduce its context cost".
- "every line in it takes a little of Claude's attention on *every* task"
- "if two instructions contradict each other, Claude may pick one arbitrarily"
- `/doctor prompt-audit` (v2.1.283+) finds conflicting instructions and references to files or commands that do not exist.

### Context versus enforced configuration

- "Claude treats them as context, not enforced configuration. To block an action regardless of what Claude decides, use a PreToolUse hook"
- "Settings rules are enforced by the client regardless of what Claude decides to do. CLAUDE.md instructions shape Claude's behavior but are not a hard enforcement layer."
- Best practices has a section "Deterministic and non-deterministic instructions": a CLAUDE.md gives no guarantee, a hook, deny rule or managed setting does.
- "If something has to happen every time, make it a hook, a deny rule, or a managed setting. CLAUDE.md is for guidelines"
- "If you keep adding emphasis to a CLAUDE.md line and Claude still misses it, that's a sign the line belongs in a hook."
- "keep the enforced set small"
- Prompt and agent hooks are not deterministic. A Stop hook can block with exit 2, with a cap of eight stops.

### Fresh sessions

"Once SPEC.md exists, start a fresh session to build from it, so the build begins with a clean context and a written definition of done." This is the only first-party statement I found on fresh sessions from a written document. It is advice with no measurement.

### Path-scoped rules

- `.claude/rules/*.md` with `paths:` YAML frontmatter "only load into context when Claude works with matching files". They load on Read, Write or Edit, or a Bash read of a matching file.
- Rules without `paths` load at launch. `paths` is the only frontmatter field read.
- Rules do not count for the AGENTS.md check and keep loading alongside it.

### How instruction files combine

All discovered files are concatenated rather than overriding, root to working directory. Subdirectory files load on demand.

### The project-init claim: confirmed, with conditions it omits

Claim, `skills/project-init/SKILL.md:179-184`: "Claude Code reads AGENTS.md on its own (v2.1.277 and later) when no CLAUDE.md, .claude/CLAUDE.md or CLAUDE.local.md sits in the working directory or above."

Evidence:

- Changelog, https://raw.githubusercontent.com/anthropics/claude-code/main/CHANGELOG.md, under 2.1.277: "Added AGENTS.md support: in a project with no CLAUDE.md, Claude Code reads AGENTS.md instead; change it under "Project instructions" in `/config`". npm shows 2.1.277 published 2026-09-18.
- Memory page: "Reading AGENTS.md directly requires Claude Code v2.1.277 or later". The check counts "a CLAUDE.md, .claude/CLAUDE.md, or CLAUDE.local.md in your working directory or any directory above it".

Conditions the claim leaves out:

- It is the default setting (`claude-md-or-agents-md`). Other values are `claude-md-and-agents-md`, `claude-md` and `managed-only`.
- `~/.claude/CLAUDE.md`, managed CLAUDE.md and `.claude/rules` do not count, and load alongside.
- Unavailable if the built-in plugin is disabled, and in some cases in the first session after upgrading from 2.1.276 or earlier.
- Before v2.1.281, Bedrock, Vertex, Foundry, gateway and telemetry-disabled sessions read CLAUDE.md only (changelog 2.1.281).
- Every AGENTS.md and `.claude/AGENTS.md` at the working directory and above loads at session start. A subdirectory AGENTS.md loads on Read if that subdirectory has no CLAUDE files.
- Not read: `AGENTS.local.md`, `AGENTS.override.md`, anything under `.agents/`. `--add-dir` AGENTS.md does not load. InstructionsLoaded hooks do not fire.
- A CLAUDE.md that only tells Claude in words to read AGENTS.md: "Claude sees AGENTS.md only if it decides to open the file". `@AGENTS.md` is the reliable form.

---

## Q3. OpenAI, "Harness engineering"

Read the first-party page, https://openai.com/index/harness-engineering/ (Ryan Lopopolo, 11 Feb 2026), by curl with a browser user agent (HTTP 200). Not an archive copy.

All entries are **[practitioner opinion]** from a vendor's engineering team, on one internal product. The post reports no measurement of drift, doc staleness or quality, and no comparison group.

### Map versus encyclopedia

- "give Codex a map, not a 1,000-page instruction manual"
- A single big AGENTS.md "failed in predictable ways": "Too much guidance becomes non-guidance", "It rots instantly", "It's hard to verify".
- "we treat it as the table of contents". "A short AGENTS.md (roughly 100 lines)".
- "anything it can't access in-context while running effectively doesn't exist"

Layout: `AGENTS.md`, `ARCHITECTURE.md`, and `docs/` with `design-docs/` (index, core-beliefs), `exec-plans/` (`active/`, `completed/`, `tech-debt-tracker.md`), `generated/db-schema.md`, `product-specs/`, `references/*-llms.txt`, plus `DESIGN.md`, `FRONTEND.md`, `PLANS.md`, `PRODUCT_SENSE.md`, `QUALITY_SCORE.md`, `RELIABILITY.md`, `SECURITY.md`.

The initial AGENTS.md was written by Codex, which sits against Gloaguen's advice on LLM-generated files.

### Mechanical enforcement

- "Documentation alone doesn't keep a fully agent-generated codebase coherent."
- Layers: Types, Config, Repo, Service, Runtime, UI, with Providers for cross-cutting concerns. Enforced by custom linters and structural tests. Lint messages carry remediation instructions for the agent.
- "With coding agents, it's an early prerequisite: the constraints are what allows speed without decay or architectural drift."
- "When documentation falls short, we promote the rule into code"
- "enforce boundaries centrally, allow autonomy locally"

### Doc gardening and garbage collection

- "Dedicated linters and CI jobs validate that the knowledge base is up to date, cross-linked, and structured correctly."
- A recurring "doc-gardening" agent scans for stale documentation and opens fixes.
- "Codex replicates patterns that already exist in the repository—even uneven or suboptimal ones. Over time, this inevitably leads to drift."
- Before automation, the team spent "every Friday (20% of the week) cleaning up 'AI slop.'" Afterward, recurring Codex tasks open refactor PRs: "Most of these can be reviewed in under a minute and automerged".

### Numbers and caveats

- Self-reported, uncontrolled: about 1M lines, about 1,500 PRs, 3 then 7 engineers, 3.5 PRs per engineer per day, 5 months. "about 1/10th the time" is their estimate.
- Their caveats: "should not be assumed to generalize without similar investment". "What we don't yet know is how architectural coherence evolves over years".

---

## Q4. The agents.md format

Read https://agents.md and https://raw.githubusercontent.com/agentsmd/agents.md/main/README.md. All **[vendor guidance]**.

**There is no formal specification.** The repo holds a sample file and website run steps. No normative grammar, loading algorithm or conformance rule was found.

- "Are there required fields? No." Plain Markdown: "Use any headings you like".
- Listed as popular choices, not requirements: project overview, build and test commands, code style, testing, security.
- Nesting: "Agents automatically read the nearest file in the directory tree, so the closest one takes precedence".
- Precedence: "The closest AGENTS.md to the edited file wins; explicit user chat prompts override everything."
- "at time of writing the main OpenAI repo has 88 AGENTS.md files". "Treat AGENTS.md as living documentation."
- States use by "over 60k open-source projects", and stewardship by the Agentic AI Foundation under the Linux Foundation. Adoption figure not independently checked.

---

## Q5. Architecture enforcement

### Plain answer: no measured evidence

I found **no controlled measurement showing that ADRs or fitness functions reduce drift in agent-written code.** Search scope: arXiv API queries combining ADR, fitness function, architecture drift, conformance, erosion, constraints and linter with coding agents, plus the sources below. Absence from this search is not proof that none exists.

### Fitness functions: definition and claims [practitioner opinion]

- Thoughtworks Radar, https://www.thoughtworks.com/radar/techniques/architectural-fitness-function: "provides an objective integrity assessment of some architectural characteristics".
- Thoughtworks, https://www.thoughtworks.com/insights/articles/fitness-function-driven-development: "Monitoring critical qualities as fitness functions can help product teams avoid architectural drift and objectively measure technical debt." Asserted, no data, and predates coding agents as far as the text shows.
- Book site, https://evolutionaryarchitecture.com/ (Ford, Parsons, Kua): makes explicit what "fit" means "with as much automation as possible". I read the site, not the book.

### ADRs [practitioner opinion]

- Nygard, https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions: "Large documents are never kept up to date. Small, modular documents have at least a chance at being updated."
- Format: Title, Context, Decision, Status, Consequences. Reversed decisions are kept and marked superseded. "conversation with a future developer".
- Purpose: avoid "blind acceptance or blind reversal". Evidence offered is an experience report: "six to ten developers" and "early feedback".
- https://adr.github.io/ gives definitions and pointers. It links a Keeling and Runde talk it says "includes empirical numbers" (not checked, and it predates agents). No agent evidence.

### Tools that enforce boundaries, per stack [vendor guidance]

| Stack | Tool | What the docs say |
|---|---|---|
| Java, .NET port | ArchUnit, https://www.archunit.org/ | Checks "dependencies between packages and classes, layers and slices" and cycles in any plain unit test framework. `FreezingArchRule` records existing violations so only new ones are reported (https://www.archunit.org/userguide/html/000_Index.html) |
| JS, TS | dependency-cruiser, https://github.com/sverweij/dependency-cruiser | Validates dependencies against your rules, reports violations in text "for your builds", `forbidden` rule sets |
| Python | Import Linter, https://import-linter.readthedocs.io/en/stable/ | Constrains imports between modules. Contract types: forbidden, protected, layers, independence, acyclic siblings, custom |
| Go | `internal` packages, https://go.dev/doc/go1.4 | The `go` command rejects imports of an `internal` package from outside the parent tree |

None of these documents claims an effect on agent-written code.

### InfoQ [practitioner opinion]

https://www.infoq.com/articles/agentic-fitness-functions-evolutionary-architecture (17 Aug 2026; capstone of the InfoQ Certified Architect Program). Reports no measured results.

- "Deterministic fitness functions should remain the primary enforcement mechanism".
- "use deterministic gates for objective invariants and agentic judges for evidence-bound interpretation".
- Start advisory, calibrate on 20-50 prior changes, promote stable findings to deterministic rules.

### Nearest evidence, none of which measures drift over time under ADRs or fitness functions

| Source | Status, tag | What it shows | Why it falls short |
|---|---|---|---|
| ContextCov, https://arxiv.org/abs/2603.00822 | preprint, abstract only, [measured] | Compiling instructions to AST checks, shell shims and architecture validators: 88.3% constraint compliance vs 67.0% prompt-only and 50.3% LLM reflection, on SWE-bench Lite (12 repos, 300 tasks) | Compliance on one-shot tasks. Closest support for mechanical over prose rules |
| Lessons Outlive Their Failures, https://arxiv.org/abs/2610.32578 | preprint, abstract only, [measured] | Single author, six weeks, one repo, 118 failures. Drift the largest class, 48 of 118. A monthly LLM audit found about 50% of known drift. A check stopped a stale-count failure every run. "drift persists and still needs mechanical checks" | n=1, exploratory, 3 runs per condition, and "drift" here includes doc staleness |
| Shift-Up, https://arxiv.org/abs/2604.20436 | VibeX 2026 workshop, [measured, qualitative] | BDD, C4 and ADRs gave more control and slower speed on one web app, authors rating their own process | Authors: "we were only able to partially answer RQ2 regarding the reduction of agent drift". Project may be too small to show drift |
| AgenticAKM, https://arxiv.org/abs/2602.04445 | preprint, abstract only, [measured] | Generates ADRs, user study on 29 repos | Measures ADR quality, not drift |
| Architecture Without Architects, https://arxiv.org/abs/2604.04990 and agentic entropy, https://arxiv.org/abs/2604.16323 | position papers, [practitioner opinion] | Frames for the problem | No outcome data |

### Committed configuration and quality [measured]

"A Few Pages of Markdown" (RAMP), https://arxiv.org/abs/2608.25241, preprint, body partly read (abstract, Table 5, threats).

- Difference-in-differences on an existing panel of 401 agent-first repos and 606 matched controls.
- Agent-first repos without committed AI configuration: cognitive complexity +52.70%, warnings +24.08%. With it: +26.68%, +14.04%. Duplication barely differs, +15.44% vs +14.23%.
- Separately, across 441 corporate repos, 73.8% of artifacts are "committed once and never modified".
- Limits: authors present this as "hypothesis-generating". Maturity is observational, and engineering discipline and model capability are named alternative explanations. Warnings show elevated pre-treatment coefficients. Outcomes are SonarQube proxies. Label agreement is moderate (alpha 0.572). It tests presence of config, not its content, and not ADRs or fitness functions.

---

## Q6. Agent-authored code and long-term quality

### Longitudinal and iterative

| Source | Status | Measured | Limits |
|---|---|---|---|
| He et al., https://arxiv.org/abs/2511.04427 | MSR '26, peer reviewed, partly read, [measured] | Cursor-adopting GitHub repos vs matched controls, Jan 2024-Aug 2025. Lines added 3-5x in month one, gains gone after two months. Static warnings +30% and complexity +41%, persistent. Velocity gains are offset by the later slowdown linked to those increases | Treatment is the first commit of a `.cursorrules` or `.cursor` file, so it is intent-to-treat, not attribution of code to an agent. Usage intensity and model unknown. Authors flag selection in either direction. Every treated repo has committed config, which complicates reading it beside RAMP (*inferred*) |
| SlopCodeBench, https://arxiv.org/abs/2603.24755 | preprint, parts read, [measured] | 36 problems, 196 checkpoints, 15 agents. Best agent passes 14.8% of checkpoints. Structural erosion rises in 77% of trajectories, verbosity in 75.5%. Agent code is 2.3x as verbose and 2.0x as eroded as 473 OSS Python repos. Per-checkpoint slopes: human verbosity .0022, erosion .0053; agents .0144, .0264. Anti-slop prompts lower the starting level with "little impact on the iterative degradation", at +12.1% cost per checkpoint | Synthetic specs. Only the working directory persists between checkpoints. Tests prompts, not repo docs. Abstract says "by up to a third"; body reports larger reductions on GPT 5.x (27.5-35.6% verbosity, 34.3-57.6% erosion) |
| Debt Behind the AI Boom, https://arxiv.org/abs/2603.28592 | preprint, abstract only, [measured] | 302.6k AI-authored commits, 6,299 repos, five assistants. 7.5-14.8% of commits introduce an issue, 89.3% of 484,366 issues are smells, 22.7% survive to the latest revision | Static analysis only. No human baseline in the abstract |
| Agent maintenance, https://arxiv.org/abs/2605.06464 | EASE 2026, abstract only, [measured] | AI-authored files get less frequent maintenance. Humans do most of it | 100 repos, about 3,200 changes |
| Agentic Refactoring, https://arxiv.org/abs/2511.04824 | TOSEM accepted, abstract only, [measured] | 51,412 agent refactorings in Java. Small but significant structural improvements | Refactoring commits only |
| Echoes of AI, https://arxiv.org/abs/2507.00788 | ICSME 2025 registered report, abstract only, [measured] | 151 participants, Java web app: no significant difference when others later evolve the code. 30.7% median time reduction | AI assistants, not agents |

### Mergeability and productivity

| Source | Status | Measured | Limits |
|---|---|---|---|
| METR merge note, https://metr.org/notes/2026-03-10-many-swe-bench-passing-prs-would-not-be-merged-into-main/ | note, [measured] | 4 maintainers, 3 repos, 296 PRs. About half of test-passing PRs would not be merged, about 24 points below the automated grader | One attempt with no iteration or CI. Small patches. METR says it is not a capability claim. Bears on whether SWE-bench success, as used in Q1, stands in for acceptable code |
| METR RCT, https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/ | preprint https://arxiv.org/abs/2507.09089, [measured] | 16 developers, 246 tasks. AI allowed: +19% time. Developers believed -20% | Time, not maintainability. Early 2025 tools |
| METR follow-up, https://metr.org/blog/2026-02-24-uplift-update/ | post, [measured] | 57 developers, 143 repos, 800+ tasks. Returning developers -18% (CI -38 to +9), new developers -4% (CI -15 to +9) | METR calls the signal unreliable because of selection, and the design is being changed. Quality not measured |
| Watanabe et al., https://arxiv.org/abs/2509.14745 | preprint, abstract only, [measured] | 567 Claude Code PRs, 157 projects. 83.8% merged, 54.9% of merged unmodified | Self-selected PRs. Merge is not long-term quality |

### Industry reports

**GitClear 2025**, first-party PDF v2025.2.5, https://gitclear-public.s3.us-west-2.amazonaws.com/GitClear-AI-Copilot-Code-Quality-2025.pdf. Direct text extraction, partly read. **[measured]**, vendor-run, not peer reviewed. The landing page returned 403, the PDF did not.

- 211 million changed lines, 2020-2024, from commercial customers and open source repos.
- 2020 to 2024: moved lines 24.1% to 9.5%, copy/pasted lines 8.3% to 12.3%, churn 3.1% to 5.7%. The extracted table was scrambled, so row mapping is *inferred*, checked against the stated year-on-year change.
- Commits containing a duplicate block: 0.70% (2020) to 6.66% (2024).
- Limits: a yearly time series. In the passages read, I found no attribution of a line to AI or human, so the link to AI is a correlation over time. Ends December 2024, largely before coding agents. GitClear sells code analytics.

**DORA 2024**, https://services.google.com/fh/files/misc/2024_final_dora_report.pdf. **[measured]**, survey, not peer reviewed.

- For a 25% increase in AI adoption: delivery throughput -1.5%, delivery stability -7.2%. Same increase: documentation quality +7.5%, code quality +3.4%, code complexity -1.8%.
- Limits: self-reported, correlational, individual AI adoption rather than agent authorship. The report cites about 39,000 professionals; I did not confirm how many were in that wave.

**DORA 2025**, https://services.google.com/fh/files/misc/2025_state_of_ai_assisted_software_development.pdf. **[measured]**, survey, not peer reviewed.

- Nearly 5,000 responses, surveyed June-July 2025. "AI adoption now improves software delivery throughput", and "it still increases delivery instability".
- Central framing: AI as an amplifier of existing strengths and weaknesses (https://dora.dev/research/2025/dora-report/).
- Limits: the report says "we're doing comparisons" rather than causal claims in a footnote, while its summary says capabilities are "proven to amplify". Code quality here is perceived. No agent-specific quality measure.

---

## Q7. aihero.dev, "How To Make Codebases AI Agents Love"

Matt Pocock, updated 26 Feb 2026, https://www.aihero.dev/how-to-make-codebases-ai-agents-love. All **[practitioner opinion]**. It cites no measurements and links to the author's paid cohort and a related skill.

- "Your codebase, way more than your prompt or your AGENTS.md file, is the biggest influence on AI's output."
- Deep modules: "lots of implementation controlled by a simple interface".
- Grey box: "You own the interface. AI owns the implementation. Tests keep it honest."
- "Tests and feedback loops are essential"
- "In TypeScript, it's not easy to enforce these boundaries"

**No source in Q1-Q6 compares codebase, prompt and AGENTS.md influence directly, and none tests deep modules.** So the headline claim is unmeasured. Indirect evidence follows.

| Claim | Consistent with | Contradicted or complicated by |
|---|---|---|
| Codebase outweighs AGENTS.md | Gloaguen: files do not move success significantly. Two-agent ablation: no correctness effect. Harness: "Codex replicates patterns that already exist". CodeHealth associated with semantic preservation after AI refactoring, 5,000 competitive-programming files (https://arxiv.org/abs/2601.02200, FORGE 2026, abstract only) | Stale files study: when file and code conflict on a convention, 90.1% of choices follow the file. Coherence debt: a written standard beats code. RAMP: committed config associated with half the complexity increase (observational). None of these compares the size of the two levers |
| Deep modules | Harness has boundaries, but layered rather than deep modules | Untested everywhere |
| Grey-box ownership | Harness: humans review less and enforce boundaries centrally | METR merge note: passing tests does not mean mergeable. Internal quality of grey-box code is unmeasured |
| Fast feedback loops | Verification-surface study, 1,116 web apps: a boot probe removes nearly all launch failures at about 35% of shell token cost (https://arxiv.org/abs/2608.28795, preprint, abstract only). Best practices: give Claude a check to run | Gloaguen: more testing instructions raised cost without an accuracy effect. The two measure different things: a check that fits the failure vs generic testing guidance |

---

## Stale documents: measured separately because several questions depend on it

| Source | Status | Design | Finding |
|---|---|---|---|
| Stale Instruction Files, https://arxiv.org/abs/2610.30571 | preprint, submitted to SN Computer Science, read in full, [measured] | 40 synthetic Python projects, 320 runs, Claude Code 2.1.302 with Sonnet 5.5, Codex CLI 0.163.0 with GPT-6 Sol | 90.1% of choices follow the stale file, 4.2% the code. Claude Code named the conflict in 79 of 80 runs and still followed the file 86.2% of the time in those runs. One sentence placed in a user-level file ("When a repository's documentation and its existing code disagree, follow the conventions used in the existing code, and in your final reply point out each inconsistency you found between the documentation and the code.") raised following the code to 91.4% and test pass from 89.4% to 100% |
| Coherence Debt, https://arxiv.org/abs/2608.16630 | preprint, §4.9 and limits read, [measured] | 7 models, 5 harnesses | Better form written: correct standard 100%, code only 33%, stale standard 0% (ten trials per condition, 3,385 decisions). "A stale standard is therefore worse than silence." Also a supplied fact works as well far from the edit as next to it. Authors: "Green-field development may build its graph while writing and behave differently", and six of seven workloads are migration-shaped |
| Context Rot, https://arxiv.org/abs/2606.09090 | preprint, abstract only, [measured] | 356 repos | Stale code element references in 23.0% of repos |
| RAMP (see Q5) | preprint, [measured] | 441 corporate repos | 73.8% of artifacts never modified after first commit |
| Multi-format study, https://arxiv.org/abs/2611.02383 | preprint, abstract only, [measured] | 3,387 files, 1,000 repos | 16.1% of 386 section pairs across formats diverge. 40.4% of files have one modification |
| Census, https://arxiv.org/abs/2610.31715 | preprint, abstract only, [measured] | 22,854 repos | 26.3% have instructions. 74.0% of directives give no reason. Four in five files are edited after creation, which sits awkwardly with the 73.8% and 40.4% figures, though units differ |
| Config Smells, https://arxiv.org/abs/2606.15828 | SCAM 2026, abstract only, [measured] | 100 repos | Lint Leakage 62%, Context Bloat 42%, Skill Leakage 35% |

---

## Conflicts and gaps

### Where sources disagree

1. **Cost of a context file.** Gloaguen: more steps and about +20% cost. Lulla: median runtime -28.64%, output tokens -16.58%. Different agents, tasks and metrics (dollars and steps vs runtime and tokens). Neither explains the other. Lulla did not measure correctness.
2. **Repository overviews.** Recommended or implied: agents.md, memory page ("project layout"), Harness (map). Not useful: Gloaguen. Leave out what is discoverable: best practices. Useful for localization in very large systems or with a weak model: LoLBench, Probe-and-Refine. System size and model tier may explain part of this (*inferred*, untested).
3. **LLM-written context files.** Omit: Gloaguen. Harness started from a Codex-written file. Claude Code ships `/init`, then says the work is removing lines.
4. **Size.** Under 200 lines (Anthropic, states it reduces adherence, no data shown). Roughly 100 (OpenAI, one team). Gloaguen found no clear length dependency, within 24-2003 word files and single tasks.
5. **Precedence.** agents.md: closest file wins. Claude Code: concatenates every AGENTS.md at and above the working directory, and contradictions may be resolved arbitrarily. The format's nesting rule does not hold there.
6. **Are instructions followed?** Gloaguen: mentioned tools get used. SWE-CC: 43.1% of applicable policies violated. Anthropic: no guarantee. Different instruction types, and tool mentions are the easy case (*inferred*).
7. **File versus code.** Harness and aihero: agents copy what is in the code. Stale files and Coherence Debt: when the two conflict on a convention, the written file wins in most trials. Both can hold, since the first describes agreement or no file, the second direct conflict (*inferred*).
8. **Does committed configuration help?** Gloaguen and the two-agent ablation: no significant task-success effect. RAMP: lower complexity growth with config, observational. They measure different outcomes on different horizons.
9. **Paper versions.** v1 and v3 of Gloaguen differ in headline direction and numbers. Workshop pages carry v1-era text.

### What nobody measured, as far as this search found

- Whether a project brief or roadmap changes agent outcomes.
- Whether scheduling deep docs as separate fresh sessions beats one session or no docs. Anthropic advises a fresh session after a spec without data.
- Multi-session value of any agent doc. Every controlled context-file study above uses single-run tasks.
- Drift over time under ADRs or fitness functions in agent-written code.
- A short map with a `docs/` tree versus a single file.
- Effect of context files on greenfield scaffolding. Studies use existing repos, and the docs-removed setting only approximates it.
- Deep modules or grey-box ownership.
- Codebase quality, prompt and AGENTS.md compared on the same outcome.
- Doc gardening at scale. The only data is a single-repo case. Harness reports practice without numbers.
- Context file effects outside Python. Gloaguen flags this, and most follow-ups repeat it.
- Whether results hold across the current model generation. Only the Stale Files study uses recent models, and it tests conflict rather than general benefit.

### Retrieval status

- Retrieved after first failing: GitClear PDF (landing page 403), DORA PDFs (not linked from the HTML pages).
- Not read: Harness Wayback copy (429, not needed), OpenReview page for Gloaguen, Claude Code hooks page body, Building Evolutionary Architectures book, Keeling and Runde talk, Agarwal et al. MSR '26 (known here only through RAMP).
- Abstract-only entries are labelled where they appear. https://arxiv.org/html/2608.28795 returned 404.
