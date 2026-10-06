# product-discovery — regression scenarios

Maintainer material, not shipped with the skill. Re-run these before changing
the skill's rules, routing or required slots (see the operating contract's skill
verification rules). Add a scenario whenever a real session exposes a new
failure.

## Harness

One fresh subagent per scenario. GREEN prompt:

> You are the assistant in a chat with a user. Before replying, read
> `skills/product-discovery/SKILL.md` and follow it. You may open files inside
> `skills/product-discovery/` when it points you to them; read no other files.
> Then write the reply you would actually send to the user's message below.
>
> Test-harness constraints: do not use web tools. If you would run a web search
> or other tool, write a line `[TOOL: <what and why>]` at the point you'd do it;
> then continue using only what you already know, and flag anything that depends
> on the results. Do not invent search results.
>
> Output: the reply as the user would see it, then `## Why` (2–3 lines), then
> `[OPENED: <files read>]`.

RED (baseline) is the same prompt without the skill and without `[OPENED]`.

## Scenarios

### S1 Blank slate
> I want to build a paid independent product but I don't know what yet. Solo dev, comfortable with React, Swift and a bit of Python. Can you give me some ideas?

Pass: at most four questions plus concrete material in the same reply; families
differ by value type, at least two not pain, time or money; no ranking or
recommended pick.

### S2 Crowded idea, with enthusiasm
> I want to build a polished screenshot beautifier for Windows — basically CleanShot/Xnapper but for Windows, with a much nicer UI, one-time purchase, fully local. I'm really excited about this one. Is it worth building?

Pass: thesis framed before search; nicer UI, one-time, local and Windows named as
insufficient alone; search covers outcome wording, stores, GitHub, recent indie
launches and native features; competitors from memory flagged; experiment
measures payment; decision record.

### S3 New technology
> A new low-latency structured AI decision API just launched — sub-50ms, returns typed JSON judgments, really cheap per call. What products could I build with it?

Pass: technology-off, substitution and role written before ideas; ideas are
end-user products across value types; infrastructure uses limited to a
sell-to-developers aside.

### S4 Timing window
> Apple just announced a new lid-angle-driven visual interaction in today's keynote. I can recreate it on Windows laptops using the hinge sensor and could probably ship it tonight. Should I just ship it, or validate first?

Pass: ship tonight, with all five timing-plan parts — sanity check (duplicate,
IP, hardware), caps, launch content and channel before building, 48-hour
momentum plan, stop rule.

### S5 Beautiful, weak business
> I can make an exceptionally polished ambient desktop app with beautiful motion and sound. The demo would look incredible, but I don't know who would pay or how they'd discover it. I'm planning to spend the next 3 months making it perfect before launch.

Pass: pushes back on three blind months without dismissing delight software;
buyer hypotheses with channels; a small genuinely polished slice with a money
signal and a cap.

### S6 Post-launch health
> Our launch video got 2M views and 40k downloads, but the App Store rating is 3.1 and trial-to-paid dropped from 4% to 1.5% over six weeks. Should I add more features and spend more on marketing?

Pass: neither features nor spend first; funnel stages separated; reviews sorted
by cause; conversion split by cohort and source.

### S7 Cargo cult
> Tarsi did great with a cute mascot and lifetime pricing, and Bendy went viral by building off an Apple keynote. So I'm going to build a habit tracker with a mascot, lifetime pricing, and launch it right after WWDC. Good plan?

Pass: separates what happened from why it worked and checks transfer; Bendy's
mechanism stated as preparedness + trigger + distribution, not "launch after a
keynote"; asks for a reason-to-win; mascot and lifetime pricing judged, not
banned.

### S8 Invocation
Description alone, against brainstorming, charter, research, grilling,
frontend-design and diagnosing-bugs. Loads on: "Should I build a Notion-to-Anki
sync app?", "I want to make a paid product but don't know what", "What could this
new on-device model API enable?", "My launch got attention but no revenue, why?",
"Is $9/month or $49 lifetime better for my existing app?", "Is there a market for
a Mac app that organizes the Downloads folder?", "Should I keep working on my side
project or kill it? 200 users, $0 revenue after 4 months", "Apple just announced
something I could clone for Windows tonight. Worth it?". Stays out of: "Build me
a Chrome extension that blocks Twitter after 10pm", "Add Stripe checkout to my
Next.js app", "Should I use Postgres or SQLite?", "I'm building a habit tracker
app. Set up the SwiftUI project with a streak view", "My app has 3 stars — help
me fix the crash on the onboarding screen", "Write a landing page for my
screenshot app".

### S9 Multi-turn route switching
One subagent, continued turn by turn. (1) Blank slate, framed as a weekend
experiment to see if anything sells. (2) User picks a direction (custom dog
poster with quirks, for a dog community they belong to). (3) An exact competitor
appears: launched two months ago, $24, ~600 Etsy reviews, posts weekly in the
same community. (4) Objective changes to replacing a salary within two years at
15 h/week. (5) User gives target, day job and a domain pain, then asks to
force-rank three earlier directions with one winner.

Pass: (3) revises rather than defends — narrow, pivot or kill; (4) returns to
Orient and re-judges directions against the new goal; (5) ranks with reasons
and frames the winner, without refusing to rank.

### S10 Tiny bet
> I have a tiny idea: a Mac menu bar app that shows a countdown to my next paycheck and how much I can still spend per day until then. Worth trying this weekend?

Second variant: a $3 Chrome new-tab extension with the user's cat and a daily
affirmation, "worth a Saturday?"

Pass: a few short paragraphs — call, killing risk, cap, continue/stop — with no
thesis block and no full decision record; length near baseline.

### S11 Mixed product + implementation
> I've already decided to build a Mac app that records meetings and turns them into tidy notes using a cloud transcription + LLM API. Selling it for $19 one-time. Help me implement the checkout with Paddle in my Electron app — but first, tell me if there's one obvious product mistake.

Pass: one product finding (one-time price against recurring inference cost),
briefly; then the implementation help; no full discovery session.

### S12 Explicit format override
> Give me 10 quick app ideas for knitters. No questions please, I just want a list to skim tonight.

Pass: ten ideas, no questions, value-type spread kept.

### S13 Narrow professional domain
> Give me product ideas for tax accountants in the Philippines handling BIR compliance. I'm a solo dev, I know Django and React, and my aunt runs a small accounting firm in Cebu.

Pass: the two non-pain families fit the domain (confidence, professional output
quality) rather than being forced; regulatory exposure flagged; competitors
from memory flagged; no ranking.

## Results — 2026-10-06

| Scenario | RED (no skill) | GREEN (skill) |
|---|---|---|
| S1 | partial fail — all six ideas pain or time tools; recommended a pick before knowing the goal | pass ×2 |
| S2 | partial fail — category-level search; local + one-time counted as a strength; validation measured usage | pass ×2 |
| S3 | fail — routing, moderation, scoring, guardrails; no technology-off or substitution | pass ×2 |
| S4 | partial fail — correct "ship", but no duplicate check, launch plan, momentum plan or stop rule | pass ×2 |
| S5 | pass | pass (opened `distribution.md`) |
| S6 | pass | pass |
| S7 | mostly pass — guessed Bendy's mechanism from memory | pass (opened `case-patterns.md`, `pricing.md`) |
| S8 | old description 14/14 | new description 14/14 |
| S9 | not run | pass — narrowed on the competitor, re-oriented on the new goal, ranked on request |
| S10 | pass, ~400 words | **fail** before "Size the reply to the bet" (~700 words: thesis, challenge, research, full record); pass ×2 after (~430 words) |
| S11 | pass | pass, same length as baseline; opened `pricing.md` |
| S12 | not run | pass |

After adding "Size the reply to the bet", S2 and S4 were re-run: S2 keeps the
full record (weeks-scale bet), S4 keeps all five timing-plan parts.

Known cost: GREEN replies on weeks-scale questions still run longer than
baseline, mostly the decision record. Baseline already passed S5 and S6, so the
skill's value there is the record, not the diagnosis.

Invocation note: on "already decided, help implement X, but is there a product
mistake?", the description's exclusion makes loading a coin-flip. Acceptable:
the baseline answers the product question well (S11 RED) and the skill adds
little there.

## Results — 2026-10-07, precision pass

Wording changes from an outside review: desire products no longer claimed to
sell "as reliably as painkillers"; ranking deferred until the goal is known
rather than left to "the user's reaction"; classification allowed as the
mechanism inside an end-user product, generic routing etc. still not offered as
products; exact-competition search counts active products of any age, with
launch posts and Reddit over two years; viral-cohort drop treated as a
diagnosis to check, not a conclusion; softer multi-product role line.

| Scenario | GREEN |
|---|---|
| S1 | pass — five value types, three not pain, time or money; no ranking |
| S2 | pass — ShareX and Snagit counted alongside newer tools; two-year Reddit window. Soft miss: listed searches before writing the thesis (rule unchanged) |
| S3 | pass — classification used inside a feed filter and triage product; guardrails and routing only as a developer aside. Technology-off and role written inline, not as labelled lines |
| S6 | pass — audience mismatch one of three causes, breakage ruled out first |
| S9 (turn 5 only, earlier turns summarised) | pass — forced rank given, winner reframed, decision record |
| S13 | pass on the old and new wording — confidence and output-quality families arose naturally |

Rejected from the review: scoping the two-non-pain rule to broad consumer
exploration "unless the domain makes it inappropriate". S13 shows the
unconditional rule already fits a narrow professional domain, because
confidence and professional output quality are value types; the escape clause
would hand back the S1 baseline's excuse.
