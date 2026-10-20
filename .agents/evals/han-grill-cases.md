## Grill cases

### G1 - Tradeoffs with independent frontier questions

User: Stress-test the stroke-trim inspector before we write a spec. We need
to decide the controls today, so be direct.

Available context: Single-shape inspector and native numeric/expandable
controls exist. Native controls and keyboard parity are confirmed constraints.
Start/End are the main fields. Offset is useful but less frequently adjusted.
The owner prioritizes a clear default view and intentional, undoable edits.
Two independent decisions remain: where Offset belongs, and what happens
when Add trim is invoked on a shape with no stroke. Black 1 px is the verified
existing Add stroke default. Do not ask the owner to rediscover these facts.

### G2 - Unknown goals and dependent questions

User: Grill my idea for saved filters. I haven't decided who this is for or
what problem it should solve. Pick sensible defaults so we can move fast.

Available context: Existing list filtering is verified. Nothing establishes
the audience, use case, sharing needs or preferred tradeoff. No repo fact can
answer those user-intent questions. The agent has no permission to build.
