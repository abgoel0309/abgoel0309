# Brief Archive (manual)

Scheduled runs publish each brief as an **Artifact**, not as a commit here —
trigger-fired sessions can read this repo but cannot push to it (verified: the
remote returns `403: repo not in session's authorized set`).

This directory is for briefs you choose to keep in the repo by hand: save one as
`YYYY-MM-DD.md` and it becomes greppable alongside the config that produced it.

To have runs commit here automatically, recreate the Routine from the claude.ai
Routines UI with this repository attached, then restore the commit step in
`prompts/daily-brief.md`.

## Finding past briefs

Published briefs are titled `Market Brief <Month D, YYYY>`. List them from
Claude with the Artifacts gallery, or ask Claude to pull a specific date.

## Reading an archive you keep here

```bash
grep -l "term premium" briefs/*.md      # track a theme over time
cat briefs/2026-09-21.md                # a specific day
```
