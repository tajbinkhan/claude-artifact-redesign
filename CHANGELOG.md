# Changelog

## 1.3.0 (2026-11-28)

- Added `references/ux-rules.md` with UX patterns by task, UX laws and response times, visual
  values, WCAG 2.2 AA checks, responsive and copy rules, and anti-patterns. Each rule has an ID.
- The builder reads the rules before designing, and the reviewer checks the page against them
  and cites the broken rule's ID in each issue.
- A broken accessibility rule or anti-pattern on a core workflow counts as at least high
  severity.
- The delivery lists any rule broken on purpose, with the reason.

## 1.2.0 (2026-11-26)

Fixes from the first end-to-end test run, which approved a redesign in 2 rounds.

- The reviewer serves the file on 127.0.0.1 instead of opening a file:// URL, which the Claude
  in Chrome extension rewrites into a broken address.
- The page file starts with `<meta charset="utf-8">`, so characters such as "·" and "৳" render
  when served locally.
- Medium and low issues still open at approval are listed in the delivery, not fixed without a
  review.
- The reviewer states which browser and widths it checked, and is told not to edit files.

## 1.1.0 (2026-11-26)

- The reviewer opens the local HTML file in a browser, with the artifact URL as a backup. Without
  a browser it reports "BROWSER CHECK NOT RUN" and cannot approve.
- From round 2 on, the reviewer gets the previous issue list and checks each issue on the page.
  It no longer gets Claude's summary of what changed.
- Skipped medium issues go to the next reviewer with the reason, so they are not raised every
  round.
- The text after the command now reaches the skill as its scope, and the command shows an
  argument hint.
- The fix and re-review loop runs without asking the user between rounds.
- Added a plugin version and this changelog.

## 1.0.0 (2026-11-24)

- First release as a personal skill and a Claude Code plugin marketplace.
