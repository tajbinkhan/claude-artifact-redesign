# ux-redesign

A Claude Code skill that redesigns an existing app's UI as an interactive Claude Artifact, then
has a fresh reviewer subagent audit it each round until the reviewer returns
`READY FOR USER VIEW`.

## Requirements

- Claude Code with the Artifact tool enabled (the Claude desktop app's Code tab or claude.ai/code).
- The skill stops if the Artifact tool is missing. It does not fall back to a local HTML file.

## Install

Clone this repo into your personal skills folder.

macOS or Linux:

```bash
git clone https://github.com/tajbinkhan/ux-redesign ~/.claude/skills/ux-redesign
```

Windows (PowerShell):

```powershell
git clone https://github.com/tajbinkhan/ux-redesign "$HOME\.claude\skills\ux-redesign"
```

To share it with one project only, clone it into `<project>/.claude/skills/ux-redesign` instead.
Start a new Claude Code session afterwards so the skill gets picked up.

## Use

```
/ux-redesign <app or screens to redesign>
```

Example: `/ux-redesign the customer inbox and ticket detail pages`

The skill sets `disable-model-invocation: true`, so Claude runs it only when you type the command.
Every round starts a new reviewer subagent, and a run can take up to 5 rounds. Delete that line
from `SKILL.md` if you want Claude to start the skill on its own.

## What it does

1. Reads the app's docs and code, and walks the running app when it can, to find the users, their
   top tasks, current UX problems and the features that must stay.
2. Designs against a fixed set of rules covering hierarchy, efficiency, consistency, WCAG AA
   accessibility, every UI state, and 375px to 1280px layouts.
3. Builds a working Artifact, with a demo-state switcher for the empty, loading and error states.
4. Starts a new `general-purpose` reviewer subagent each round. The reviewer uses the page in a
   browser, rates 10 areas, ranks every issue by severity, and ends with `READY FOR USER VIEW` or
   `NOT READY`. Claude fixes every critical and high issue and republishes.
5. Stops on approval, or after 5 rounds with the open issues listed. Claude never approves its own
   work.

## License

MIT
