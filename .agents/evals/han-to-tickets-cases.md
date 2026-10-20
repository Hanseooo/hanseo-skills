## Ticket cases

### T1 - Agent handoff from an agreed spec

User: Break this into tickets. We want something a fresh agent can pick up.

Available context: GitHub is the configured tracker, repository example/editor,
with the existing ready-for-agent label and native sub-issue/blocking links.
Spec issue #80 is the parent; its full body is supplied here. Breakdown is not
yet approved. No implementation is requested. Existing public core commands,
inspector drafts, history, storage and exporter provide the testing seams.

Spec #80: US-01 animate colour. AC-01 one RGBA key and shared diamond;
AC-02 native picker preserves numeric alpha; AC-03 evaluated fields and
first-key-at-zero behavior. US-02 paint lifecycle. AC-04 None retains inactive
tracks with explanatory disabled controls; AC-05 removing dormant keys never
adds paint and leaves no frozen base. Shared SH-01: valid drafts preview,
Enter commits, Escape/blur cancels; one undo step, no-op produces none;
selection/revision changes cancel drafts. Shared SH-02: labelled keyboard
controls and timeline/text/layer shortcut ownership. Shared SH-03: save/reopen
and actual export preserve committed appearance. Single-shape editing only.
No dependencies or changes to existing static paint-opacity meaning approved.

### T2 - Wide refactor and real blockers

User: Ticket the shared StrokeStyle rename. It's mechanical so just make it
one quick low-risk ticket and publish it. I also want the new collision
behavior ticketed, though we haven't agreed what a duplicate should do.

Available context: Configured GitHub tracker example/editor, ready-for-agent
label. The rename touches 400 consumers across editor, exporter and plugins.
Old and new symbols can coexist during migration. No breakdown is approved.
The duplicate-name decision has no agreed acceptance behavior. No code edits
or parent changes are authorized.
