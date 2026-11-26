# claude-artifact-redesign

A Claude Code skill (`/claude-artifact-redesign`) that redesigns an existing app's UI as an interactive Claude Artifact, then
has a fresh reviewer subagent audit it each round until the reviewer returns
`READY FOR USER VIEW`.

## Requirements

- Claude Code with the Artifact tool enabled (the Claude desktop app's Code tab or claude.ai/code).
- The skill stops if the Artifact tool is missing. It does not fall back to a local HTML file.

## Install

### As a plugin (recommended)

Run these in Claude Code:

```
/plugin marketplace add tajbinkhan/claude-artifact-redesign
/plugin install claude-artifact-redesign@claude-artifact-redesign
```

Updates arrive through `/plugin` like any other marketplace plugin.

### As a personal skill

Clone this repo into your personal skills folder.

macOS or Linux:

```bash
git clone https://github.com/tajbinkhan/claude-artifact-redesign ~/.claude/skills/claude-artifact-redesign
```

Windows (PowerShell):

```powershell
git clone https://github.com/tajbinkhan/claude-artifact-redesign "$HOME\.claude\skills\claude-artifact-redesign"
```

To share it with one project only, clone it into `<project>/.claude/skills/claude-artifact-redesign` instead.
Start a new Claude Code session afterwards so the skill gets picked up.

## Use

```
/claude-artifact-redesign <app or screens to redesign>
```

Example: `/claude-artifact-redesign the customer inbox and ticket detail pages`

Installed as a plugin, the command carries the plugin name as a prefix:
`/claude-artifact-redesign:claude-artifact-redesign`.

The skill sets `disable-model-invocation: true`, so Claude runs it only when you type the command.
Every round starts a new reviewer subagent, and a run can take up to 5 rounds. Delete that line
from `SKILL.md` if you want Claude to start the skill on its own.

The loop runs without asking you between rounds. Claude Code can still ask you to approve
individual tool calls, such as publishing the artifact, depending on your permission settings.

The reviewer needs a browser tool (Claude in Chrome or the desktop app's built-in browser) to
approve the design. Without one, the skill stops after a retry and tells you why.

## What it does

1. Reads the app's docs and code, and walks the running app when it can, to find the users, their
   top tasks, current UX problems and the features that must stay.
2. Designs against a fixed set of rules covering hierarchy, efficiency, consistency, WCAG AA
   accessibility, every UI state, and 375px to 1280px layouts.
3. Builds a working Artifact, with a demo-state switcher for the empty, loading and error states.
4. Starts a new `general-purpose` reviewer subagent each round. The reviewer serves the local
   HTML file on 127.0.0.1, uses it in a browser, checks the previous round's issues, rates 10
   areas, ranks every issue by severity, and ends with `READY FOR USER VIEW` or `NOT READY`.
   Claude fixes every critical and high issue and republishes.
5. Stops on approval, or after 5 rounds with the open issues listed. Claude never approves its own
   work. Medium and low issues still open at approval are listed in the final report.

The reviewer starts its server with `python -m http.server`, so Python on the PATH helps. Without
it, the reviewer falls back to the artifact URL.

## License

MIT
