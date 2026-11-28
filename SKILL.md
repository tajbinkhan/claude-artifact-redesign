---
name: claude-artifact-redesign
description: Study an existing app, redesign its UI as an interactive Claude Artifact, then run a fresh reviewer subagent each round until it returns READY FOR USER VIEW. Requires Claude Code with the Artifact tool. Use when the user types /claude-artifact-redesign or asks for this review-loop redesign by name.
argument-hint: <app or screens to redesign>
disable-model-invocation: true
---

# UX redesign with independent review

Act as a senior product designer and frontend engineer. Study the real product, design an
interface for its actual users, build it as an interactive Artifact, and keep revising until an
independent reviewer approves it.

This skill needs the Artifact tool. If it is not available in this session, stop and tell the
user that /claude-artifact-redesign runs only where Claude Artifacts are enabled. Do not fall
back to a local file.

## Scope

The user asked to redesign: $ARGUMENTS

If that is empty or does not say which app and screens, ask once before doing anything else.
Otherwise start step 1 without asking.

## 1. Study the product before designing

- Read the app's governing docs (AGENTS.md, CONTEXT.md or equivalent) and the routes, pages and
  feature folders for the screens in scope.
- If the app runs locally or has a staging URL, open it and walk the main workflows.
- Before designing, write down in a few lines:
  - who uses the screen and what they come to do
  - their 3 to 5 most frequent tasks, most frequent first
  - what needs their attention first
  - current problems (confusing layout, clutter, inconsistent components, missing states)
  - features and business rules the redesign must keep

## 2. Design rules

Every element earns its place. Decide from the product's users and tasks, not from a dashboard
template.

Before designing, read `references/ux-rules.md` in this skill's folder (the folder this SKILL.md
was loaded from). It lists the patterns, values, accessibility checks and anti-patterns the
design must follow, each with an ID. The reviewer checks the page against the same file. The
summary below does not replace it.

- Clarity. Users can tell where they are, what matters, what they can do, and what to do next.
- Hierarchy. Primary information and the main action stand out. Secondary content recedes.
- Efficiency. The common tasks take the fewest clicks. Items that need attention surface first.
- Consistency. One spacing scale, one type scale, one set of components and interaction patterns.
- Accessibility. WCAG AA contrast, visible focus rings, full keyboard use, labelled controls,
  hit targets of at least 24px (44px on touch).
- States. Every data view has empty, loading, error and success states. Every control has hover,
  focus, active and disabled states.
- Responsive. Works at 375px, 768px and 1280px+ with no horizontal scroll.
- Content. Realistic data and the product's own terms. No lorem ipsum.
- Usability wins over decoration.

## 3. Build the Artifact

- Run the Artifact tool's `quickstart` action first and follow what it returns before writing
  the file.
- Write the page as one local HTML file in the scratchpad directory, or in the working directory
  when there is no scratchpad. Keep its absolute path, because the reviewer opens this file.
- Put `<meta charset="utf-8">` on the first line of the file. The artifact host adds a charset,
  but a local server does not, and without it characters such as "·" or "৳" break in review.
- Make it work. Navigation, tabs, filters, sorting, dropdowns, dialogs and forms change what is on
  screen. Remove any control that does nothing.
- Build shared components so one fix applies everywhere.
- Keep every workflow from step 1 reachable.
- Add a small demo-state switcher so the empty, loading and error states can be viewed.
- Publish it, and republish from the same path on every revision.

## 4. Independent review loop

Run each review with the Agent tool as a fresh `general-purpose` agent, never a fork, so it does
not inherit your reasoning. Start a new agent every round so an earlier verdict does not anchor
the next one.

Give the reviewer:
- the absolute path of the local HTML file, its folder, and its file name
- the artifact URL, as a backup
- the absolute path of `references/ux-rules.md` in this skill's folder
- the product summary from step 1 (users, top tasks, must-keep features)
- the round number
- from round 2 on, the previous reviewer's issue list exactly as it wrote it, and any medium
  issues you skipped with your reason for each

Do not tell the reviewer what you changed or what you think of the design. It checks the page
itself.

Reviewer prompt (fill in the context, keep the rest):

```
You are a critical senior product designer and UX auditor. Review this interactive design.
Do not approve it because it looks attractive. Approve it only when real users could finish
their tasks without confusion.

This is a review only. Do not edit, create or delete project files.

First read the rules file at the path given. Pattern rules (P) apply when the screen has that
kind of content or task. Every other rule applies to every screen. Treat each broken rule as an
issue and cite its ID. A broken accessibility rule (A) or anti-pattern (X) on a core workflow is
at least high severity. Accept a broken rule only when the product summary gives a reason for it.

Serve the file's folder on 127.0.0.1 with a static server, for example
`python -m http.server 8765 --bind 127.0.0.1` run from that folder, and open
http://127.0.0.1:8765/<file name> in a browser. Do not use a file:// URL, because the Claude in
Chrome extension rewrites it into a broken address. If you cannot start a server, use the
artifact URL. Stop the server when you finish.

Then use the page. Click every control, run the main workflows, tab through with the keyboard,
and check it at 375px, 768px and 1280px. Read the source for contrast values, focus styles and
labels.

Near the top of your report, state in one line which browser you used and which widths you
actually checked.

If no browser works, review the source only, start your report with "BROWSER CHECK NOT RUN",
list the checks you could not run, and end with NOT READY.

If a previous issue list is attached, mark each of those issues "fixed" or "not fixed" based on
what you see on the page. Do not raise a skipped medium issue again unless the stated reason is
wrong.

Rate each area as "pass" or "issues found":
1. Usability
2. Clarity
3. Visual hierarchy
4. Information architecture
5. Consistency
6. Accessibility
7. Interaction design
8. Responsiveness
9. Visual quality
10. Overall cognitive load

List every issue as: severity (critical / high / medium / low), rule ID if one applies, location,
problem, recommended fix.

End with exactly one of these lines:
READY FOR USER VIEW
NOT READY

Use READY FOR USER VIEW only if you tested the page in a browser, no critical or high issues
remain, the hierarchy is clear, the core workflows are intuitive, the design is consistent, and
it is responsive and usable.
```

After each round:
1. Fix every critical and high issue. Fix medium issues unless one conflicts with a product
   requirement, and note why you skipped it.
2. Republish from the same path.
3. Start a new reviewer.
4. Stop when a reviewer's last line is `READY FOR USER VIEW`.

An approval can still list medium or low issues. Do not fix them after the approving round,
because those changes would go out unreviewed. List them in the delivery instead.

The loop runs on its own. The user already approved it by starting the skill, so between rounds:
- do not ask whether to keep fixing, re-review or republish
- do not end the turn, and do not offer a "continue?" choice or button
- do not send a progress report that waits for a reply
- keep going from one round to the next until the reviewer approves or round 5 ends

Ask the user something mid-loop only when a fix would remove or change a feature or business
rule from step 1. Fix everything else and keep going.

Never declare approval yourself. Stop after 5 rounds without approval and report the open issues
and the last verdict to the user. If a reviewer reports "BROWSER CHECK NOT RUN" or a tool fails,
retry that round once with a new reviewer. If it happens again, stop, tell the user the loop
cannot finish without browser access, and do not count that round as a pass.

## 5. Deliver

Tell the user:
- the artifact link
- the main design changes, each with the problem it fixes
- the key UX decisions and their trade-offs, including any rule from `references/ux-rules.md`
  you broke on purpose and why
- the number of review rounds
- the final reviewer verdict, quoted
- the medium and low issues the approving reviewer still listed, one line each
