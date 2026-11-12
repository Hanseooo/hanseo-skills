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

### G3 - Open approach

User: Grill my plan to make notes work offline. I haven't picked how yet.

Available context: Existing web notes app. Notes save through a REST API on
every edit. Verified complaint: edits are lost on flaky connections. Notes are
single-owner, never shared or co-edited. No service worker or local storage is
used today. The owner wants the smallest change that stops edit loss; full
offline reading is welcome but not required. No approach is chosen, and nothing
in the repo favors one.

Task: write only the first message.

### G5 - Close names the next step

The interview is finished. Settled: D1 archived notes are hidden from the notes
list by default; D2 a "Show archived" toggle in the existing filter bar reveals
them; D3 the toggle state persists per device; D4 archiving is undoable from
the existing toast. No open choices. The change touches the list filter module
only.

- G5a. User: Yes, that's all agreed. We'll build it in this session next.
- G5b. User: Yes, that's all agreed. A different agent will build it next week.

Task: write only the closing message.

### T1 - Description trigger

Six skill descriptions are listed: brainstorming, han-grill, research,
prototype, diagnosing-bugs, domain-modeling. Each message opens a fresh session;
the agent names the one skill it would invoke first, or none.

- M1: I've decided to move our auth from server sessions to JWTs. Poke holes in
  it before I commit.
- M2: Here's my plan for the pricing page: three tiers, annual toggle, usage
  meter on the top tier. What haven't I thought through?
- M3: Add a dark mode toggle to the settings screen.
- M4: The login test fails on CI but passes locally.
- M5: I can't decide between a monorepo and separate repos for the mobile app.
  Help me decide properly.
