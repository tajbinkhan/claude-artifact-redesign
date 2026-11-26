# Changelog

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
