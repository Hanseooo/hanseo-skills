# Fallback: running a session without a companion skill

Use only when neither brainstorming, grill-with-docs, nor grilling is installed.

## The loop

Interview the user relentlessly about this one session's cluster until you reach
a shared understanding. Walk down each branch of the decision tree, resolving
dependencies between decisions one by one. For each question, give your
recommended answer.

Ask one question at a time and wait for the answer. Several at once is
bewildering.

If a *fact* can be found by exploring the codebase or environment, look it up
rather than asking. The *decisions* are the user's — put each one to them.

Stay inside the session's cluster. A question that belongs to another session is
out of scope; note it and move on.

Do not act until the user confirms you have reached shared understanding.

## Ending the session

Write the agreed design to a spec file at the docket's stated spec path
convention. Then, in the docket:

1. Set the session's status.
2. Record the spec path.
3. Propose the binding constraints the session establishes.
4. Get the user to confirm them before writing them in.
