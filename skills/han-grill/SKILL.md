---
name: han-grill
description: Stress-test an idea through a decision-tree interview with explicit tradeoffs and grounded recommendations.
disable-model-invocation: true
---

# han-grill

Reach a shared understanding by probing consequential choices, comparing
alternatives and following their consequences. This independent interview
requires no companion skill and produces an agreement in the conversation.

## Work the decision tree

1. Read the idea, relevant project evidence and prior answers. Briefly reflect
   the intended outcome and known constraints. Keep settled decisions separate
   from assumptions and unknown facts. A clear brief needs no repeated intake.
2. Map decisions and their prerequisites. The **frontier** contains choices
   whose prerequisites are already settled. Ask the independent frontier
   questions in one numbered round, then wait. Honor a requested question pace.
   Sharing or implementation choices dependent on an unknown audience/job wait
   for that answer; unknown intent calls for an open question.
3. Investigate discoverable facts yourself. Use independent fact-finding
   subagents when available and permitted; otherwise inspect directly. A
   running investigation blocks its dependent questions, not the rest of the
   frontier. Access limits remain explicit rather than becoming guessed facts.
4. After each answer, update the tree and test consequences through the user
   journey, relevant failure/recovery states, keyboard/focus, data and system
   boundaries. Challenge contradictions with earlier constraints. Explore
   consequential branches without introducing speculative product scope.

## Make choices understandable

For a decision with meaningful alternatives, use this question format:

```markdown
❓ **Q1** - **<title>**: <decision and why it matters>

A. <option>
- Benefit: <concrete user/project consequence>
- Cost: <concrete drawback or tradeoff>

B. <option>
- Benefit: <concrete user/project consequence>
- Cost: <concrete drawback or tradeoff>

➡️ **Recommendation:** <offered option and the known goal, constraint or
verified fact that favors it>
```

Usually offer two or three viable options. Describe their actual user-facing
behavior, including recovery where it changes the choice. Keep benefits and
costs equally specific; a weaker option must still be a plausible choice.
When no material drawback is known, say so rather than inventing one.

For example, an expandable Offset field reduces default inspector clutter but
adds a step to adjustment. An always-visible field improves access and scanning
but adds density. A preference for a clear default view supports the former;
frequent Offset work could favor the latter.

Recommendations follow evidence. If no known preference or verified fact
distinguishes the options, replace Recommendation with **Decision help:** what
information would determine the choice. For unknown goals or needs, ask an
open question and explain its significance. Discover those before constructing
downstream alternatives. The option template applies to choices, not every
question.

Invite choosing, combining, proposing another approach or delegating. Make the
recommended action explicit so "yes" can accept it. Clarify an ambiguous reply
to a multi-question round before recording decisions. Suggestions and silence
are not agreement. Honor explicit delegation where enough context exists to
make the choice; record the reason and any remaining assumptions.

## Close the agreement

The interview is complete when the consequential frontier is resolved and the
user confirms shared understanding. Recap the outcome, scope, agreed behavior,
decisions and rationale. Keep unresolved choices and what they block visible
if the user pauses or ends early. Reopen settled choices for material new
evidence or changed requirements, explaining the conflict first.

Stop at the interview boundary. Writing a spec, creating tickets, implementing,
committing and publishing are separate requests. The recap can be input to
`/han-to-spec` or any other workflow; none is automatically invoked.
