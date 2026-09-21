# Daily Market Brief

An automated daily research brief covering **equities, crypto, rates & credit,
and macro** — synthesized each weekday morning from public primary sources,
government data, institutional research, news, and forum sentiment.

A scheduled Claude session runs before the US open, sweeps the source universe
live, writes the brief, scores its own prior calls, and commits everything here.

---

## How it works

```
  6:00 AM ET, Mon–Fri
          │
          ▼
  ┌─────────────────┐
  │  Claude Routine   │  fresh session, live web access
  └─────────┬─────────┘
            │  clones this repo (read-only)
            ▼
  prompts/daily-brief.md ──── the procedure
  config/sources.md ───────── what to sweep
  config/brief-template.md ── structure + rigor rules
  THEMES.md ─────────────── seed standing questions
  previous brief (Artifact) ─ open theses + scorecard
            │
            │  sweeps ~25–40 sources across 4 tiers
            ▼
  publishes:  Market Brief <date>   — an Artifact, stable URL
  notifies:   push + email summary
```

No API keys, no secrets, no CI minutes — the Routine runs on your existing
Claude access.

### Why the output is an Artifact and not a commit

Scheduled sessions can **read** this repo but cannot **write** to it: a
trigger-fired session carries no attached repo source, so `git push` returns
`403: repo not in session's authorized set`. This was verified with a
diagnostic run, not assumed.

So the repo holds the **inputs** — the runbook, the source list, the standards —
and each day's **output** is a published Artifact with its own URL. Continuity
works by reading the previous day's brief rather than a committed file, which is
why section 11 restates every open thesis in full instead of referring back.

To get briefs committed here instead, create the Routine from the claude.ai
Routines UI with this repository attached — that grants the write access the
MCP-created trigger can't.

## What's in a brief

Twelve sections, defined in `config/brief-template.md`:

| # | Section | What it's for |
|---|---|---|
| 1 | The one-liner | State of the world in a sentence |
| 2 | What matters today | 3–5 ranked items, each with a falsifier |
| 3 | Macro | Growth, inflation, labor, fiscal, policy path |
| 4 | Rates & credit | Curve, decomposition, auctions, spreads, funding |
| 5 | Equities | Breadth, dispersion, earnings, vol, flows |
| 6 | Crypto | Flows, funding, stablecoins, on-chain, regulation |
| 7 | Cross-asset & divergences | Where markets tell inconsistent stories |
| 8 | Sentiment & positioning | Forums, surveys, prediction markets |
| 9 | Calendar ahead | Next 1–5 sessions, signal vs noise |
| 10 | What consensus may be missing | Contrarian, high bar |
| 11 | Scorecard | Prior calls: tracking / broken / resolved |
| 12 | Coverage log | What was consulted, what was missed |

Sections 7, 10 and 11 are where the value concentrates. Sections 1–6 could be
assembled from any news aggregator; the cross-asset synthesis, the contrarian
read, and the honest scoring of prior calls are what a digest can't give you.

---

## Source tiers

The brief treats sources differently depending on what kind of evidence they are
— this is enforced in the analysis standards, not just a filing convention.

| Tier | What | How it's treated |
|---|---|---|
| **1** | Fed, BLS, BEA, Treasury, SEC, CFTC, ECB/BoE/BoJ/PBoC, BIS, IMF, EIA | Fact |
| **2** | Regional Fed research, NBER, Brookings, PIIE, Apollo, BlackRock, JPM, GS, MS, PIMCO, Reuters/FT/WSJ/Bloomberg, CoinDesk/The Block | Informed interpretation, attributed |
| **3** | Reddit, Bogleheads, HN, FinTwit, Stocktwits, protocol governance forums, Polymarket/Kalshi | Evidence about *belief*, never about the world |

Full list with links: `config/sources.md`.

---

## The rules that keep it honest

Defined in `config/brief-template.md`. The important ones:

- **No fabricated numbers.** Every figure comes from a source checked that run.
  `[unverified]` is always available; a remembered level is not a quote.
- **Observation separated from interpretation**, structurally.
- **Every call carries a falsifier** — the specific thing that would prove it
  wrong, written concretely enough for a later brief to check.
- **Tier 3 stays Tier 3.** "Reddit is convinced of X" is a positioning datapoint,
  not information about X.
- **Gaps get logged.** Uniform coverage every single day would be a lie about
  at least some of those days.
- **Views get committed to.** "Markets may rise or fall" is not caution.

---

## It has a memory

Every brief carries open theses with concrete falsifiers and a scorecard of what
held up and what broke — including the misses, deliberately. Each run reads the
previous brief and scores those calls against the day's evidence.

Because the run can't write back to this repo, **the brief is the memory**:
section 11 restates each open thesis in full (claim, mechanism, falsifier,
confidence) so the next run can pick it up cold. `THEMES.md` here is the seed —
standing questions and anything you add by hand.

This is what separates the archive from a pile of daily notes. Without the
scorecard there's no feedback loop and the brief never gets better at anything.

## Repo layout

```
prompts/daily-brief.md       Execution runbook — the steps a run follows
config/sources.md            Source universe, tiered, with links
config/brief-template.md     Analysis standards + 12-section structure
THEMES.md                    Seed standing questions; hand-edited theses
briefs/                      For briefs you choose to archive by hand
```

## Changing it

Everything is plain Markdown read fresh on each run — edit a file, and the next
brief reflects it. No redeploy.

| To change | Edit |
|---|---|
| What gets covered | `config/sources.md` |
| Output structure or length | `config/brief-template.md` |
| How the run operates | `prompts/daily-brief.md` |
| What it's tracking | `THEMES.md` |
| Schedule | The Routine, via Claude |

**Adding a watchlist:** add a section to `config/brief-template.md` listing your
tickers and a dedicated section in the output structure. The run will pick it up
the next morning.

**Archiving to the repo:** briefs are published as Artifacts, not committed.
Either recreate the Routine from the claude.ai Routines UI with this repo
attached, or paste briefs you want to keep into `briefs/YYYY-MM-DD.md`.

**Schedule:** currently `0 10 * * 1-5` UTC = 6:00 AM ET during daylight time.
UTC cron doesn't follow DST, so this becomes 5:00 AM ET in winter. Ask Claude to
shift it to `0 11 * * 1-5` in November to hold 6:00 AM, or leave it and read it
an hour earlier.

---

## Scope

Research and analysis, not investment advice. Automated synthesis of public
sources gets things wrong — the falsifiers and the scorecard exist precisely
because that's expected. Verify anything you'd actually trade on against the
primary source, which the brief links for exactly that reason.
