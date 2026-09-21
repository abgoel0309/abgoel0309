# Brief Structure & Analysis Standards

Two things live here: the **shape** every brief takes, and the **rules** that
keep it honest. Both are read fresh on each run, so editing this file changes
the next brief.

---

## Part 1 — Analysis standards

These are non-negotiable. A brief that breaks them is worse than no brief,
because it launders guesses into something that reads like research.

### Never fabricate a number
Every price, yield, spread, or flow figure must come from a source checked on
this run. If a figure can't be verified, write `[unverified]` or omit the
sentence. An approximate level explicitly labeled as approximate is fine; a
precise-looking invented number is not. This applies with equal force to things
that feel obviously true — a level you "know" from training is a memory, not a
quote, and markets have moved since.

### Separate observation from interpretation
Structurally, not just tonally. State what happened, then say what you think it
means, and make it obvious which is which. "The 10y rose 8bp to X%" and "that
looks like term-premium rebuild rather than a hawkish repricing" are different
kinds of claim and should not be welded into one sentence.

### Attach confidence and falsifiers
Any non-trivial interpretation gets a confidence level (high / medium / low) and
a **falsifier** — the specific observation that would prove it wrong. An
interpretation with no falsifier is a vibe. Write the falsifier concretely
enough that a future brief can check it: "2s10s re-inverts" beats "sentiment
deteriorates."

### Tier your sources
Tier 1 is fact. Tier 2 is informed interpretation, attributed by name. Tier 3 is
sentiment data — evidence about what market participants believe, never evidence
about the world. Never promote a Tier 3 claim to a factual one. "r/investing is
convinced a cut is coming" is a real and useful datapoint about positioning; it
is not information about what the Fed will do.

### Say when you don't know
Gaps are information. An unreachable source, a thin news day, a data release
that hasn't landed yet — log it. A brief that pretends to uniform coverage
every single day is lying about at least some of those days.

### Resist recency bias
One session is noise more often than it is signal. Explicitly distinguish
"today's move" from "the trend this move sits inside." When something looks
like a regime break, say what would confirm it over the following week rather
than declaring the break on day one.

### Steelman the other side
Every directional view gets the strongest available counter-argument, stated
in its best form rather than as a strawman to knock down. If the counter is
genuinely weak, say why — but do the work first.

### No hedge-everything mush
Rigor is not the same as refusing to commit. "Markets may go up or down
depending on data" is not caution, it's noise. Take positions, qualify them
honestly, and let the scorecard hold you to them.

---

## Part 2 — Brief structure

Filename: `briefs/YYYY-MM-DD.md`. Sections in this order.

### Front matter
```
---
date: YYYY-MM-DD
session_covered: <prior US close through this morning>
generated: <ISO timestamp> ET
---
```

### 1. The one-liner
A single sentence: the state of the world this morning. If you can't compress
it to one sentence, you haven't finished thinking about it.

### 2. What matters today (3–5 items)
The ranked list. Each item:
- **What happened** — fact, with source link
- **Why it matters** — the transmission mechanism, not just "it's bullish"
- **What would change this read** — the falsifier
- **Confidence** — high / medium / low

Rank by *impact on the forward path*, not by headline volume. A quiet technical
change in Treasury refunding composition can outrank a loud earnings headline.

### 3. Macro
Growth, inflation, labor, fiscal, policy path, geopolitics. New data against
expectations, and — more importantly — against the *trend*. Central bank
communication and what shifted in the market-implied path. Nowcast updates
(GDPNow, Cleveland Fed inflation). Political and geopolitical developments with
a real market transmission channel; skip the ones without.

### 4. Rates & credit
Curve levels and shape. What drove the move: real yields, breakevens, or term
premium — decompose rather than just reporting the nominal. Auction demand
(bid-to-cover, tails, dealer takedown) when relevant. Credit spreads and
issuance. Funding markets (SOFR, repo, basis-trade leverage, swap spreads).
Global rates when they're driving the US leg. MOVE and rate vol.

### 5. Equities
Index levels and the shape underneath: breadth, sector dispersion, factor
rotation, equal-weight vs cap-weight. Earnings — results, guidance, and the
revision trend. Valuation context relative to rates. Vol surface: VIX term
structure, skew, positioning. Flows: buybacks, retail, systematic, fund flows.
Single names only when they carry macro information.

### 6. Crypto
Majors and the market structure underneath: ETF flows, funding rates, open
interest, liquidations. Stablecoin supply as the liquidity proxy. On-chain
behavior — exchange balances, long-term holder distribution. Regulatory and
legislative developments (often the dominant driver). Protocol and governance
events, security incidents, major unlocks. Correlation regime with equities and
liquidity — say explicitly whether crypto is trading as a risk asset, a
liquidity proxy, or on idiosyncratic news.

### 7. Cross-asset & divergences
The section that earns the brief's keep. Where are assets telling
**inconsistent stories**? Equity vol calm while rate vol screams. Credit tight
while small caps break down. Gold and real yields both rising. Crypto decoupled
from Nasdaq. For each divergence: which market is usually right in this setup,
and what resolution would look like. Note when a historical relationship has
stopped working — that is itself the signal.

### 8. Sentiment & positioning
Explicitly Tier 3, explicitly labeled. Forum temperature across equity, macro,
rates, and crypto communities: what retail believes and how fast it's changing.
Fund manager surveys and institutional positioning. Prediction market odds with
levels and moves. Put/call, AAII, CNN Fear & Greed, crypto Fear & Greed.
Crowding flags — where consensus has become uncomfortably one-sided. Note where
sentiment and price have *diverged*, which is usually where this section pays.

### 9. The calendar ahead
Next 1–5 sessions: data releases with consensus, central bank speakers, auctions,
earnings, policy deadlines, known crypto events (unlocks, upgrades, rulings).
Flag which of these could actually move the tape versus which are noise.

### 10. What consensus may be missing
One or two genuinely contrarian observations. The bar is high — this is not a
section for reflexive contrarianism. Either something under-covered relative to
its importance, a consensus assumption that looks fragile, or a second-order
effect nobody has traced through. If nothing clears the bar on a given day,
write "nothing clears the bar today" and move on. That's an honest answer and
far better than manufacturing a hot take on schedule.

### 11. Scorecard
Check open calls from `THEMES.md`: what's tracking, what's broken, what
resolved. Update `THEMES.md` accordingly in the same run. This is the section
that makes the archive compounding rather than disposable — without it, the
brief has no memory and never learns.

### 12. Coverage log
Sources actually consulted this run, grouped by tier. Sources that were
unreachable or paywalled. Any section with thinner-than-usual coverage, stated
plainly.

---

## Length and voice

Target 1,200–2,000 words. Dense and skimmable: bold the claim, let the
supporting detail follow. Bullets for scannable facts, prose for reasoning —
reasoning compressed into bullets loses the logical connective tissue that makes
it checkable.

Write like a sharp analyst briefing a colleague who is smart, busy, and will
notice if you're bluffing. No hype, no filler transitions, no restating the
section header as a sentence. If a section is genuinely quiet, two lines saying
so beats four paragraphs of padding.
