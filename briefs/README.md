# Brief Archive

One file per trading day: `YYYY-MM-DD.md`. `latest.md` is a copy of the most
recent brief, so there's a stable path to read or link.

Written by the scheduled daily run — see `prompts/daily-brief.md` for the
procedure and `config/brief-template.md` for the structure.

## Reading the archive

```bash
# Today
cat briefs/latest.md

# Track a theme across time
grep -l "term premium" briefs/*.md

# What was said about a date
cat briefs/2026-09-21.md

# How a thesis evolved
grep -A3 "T-001" briefs/*.md
```

The archive is meant to be grepped. When a market move surprises you, the
useful question is usually "what did I think about this two weeks ago" — and
that only has an answer if the history is intact. Don't prune it.
