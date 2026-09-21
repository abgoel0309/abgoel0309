# Running Themes & Scorecard

The brief's memory. Without this file each day starts cold and nothing
compounds; with it, the archive gets more useful over time because you can see
which reads held up and which didn't.

**How this file is used.** Scheduled runs can read this repo but cannot write to
it, so a run does *not* update this file. It reads it as the seed — the standing
questions below, plus any thesis you add by hand — and carries the live thesis
log forward inside each published brief's section 11 instead.

Edit it by hand whenever you want to plant, kill, or reweight a thesis; the next
morning's run will pick up the change.

---

## Open theses

Each entry follows this shape. The falsifier is the important field — a thesis
you can't disprove isn't a thesis.

```
### T-NNN · <short name>
- **Opened:** YYYY-MM-DD
- **Claim:** <one sentence, specific enough to be wrong>
- **Why:** <the mechanism, not the vibe>
- **Falsifier:** <the concrete observation that kills this>
- **Confidence:** high / medium / low
- **Status:** tracking / strained / broken
- **Log:**
  - YYYY-MM-DD — <what happened, and what it did to the thesis>
```

*No open theses yet. The first daily run will populate this section.*

---

## Resolved

Closed theses, correct and incorrect, with what actually happened and what the
error was in the ones that missed.

**Keep the misses.** A scorecard that only records wins is marketing, not
analysis, and it removes the only feedback loop this system has.

*Empty until the first thesis resolves.*

---

## Standing questions

Open questions that aren't directional calls but shape how the brief reads
incoming data. These persist across many briefs and get revisited as evidence
accumulates.

Seed set — revise freely as the picture changes:

1. **Policy path** — Where is the terminal rate and what is the market
   mispricing about the path to it?
2. **Inflation persistence** — Is the services/shelter component decelerating
   on trend, or is the disinflation now mostly goods-driven and fragile?
3. **Labor market** — Is the softening a normalization to trend or the early
   part of a nonlinear deterioration? What distinguishes the two in the data?
4. **Term premium** — How much of long-end yield is supply/fiscal-driven versus
   growth and inflation expectations?
5. **Equity concentration** — Is index-level strength broadening or still
   carried by a handful of names? What breaks the concentration trade?
6. **Credit cycle** — Are spreads compensating for the default outlook, and
   where is private-credit stress showing up first?
7. **Crypto regime** — Is crypto trading as a liquidity proxy, a risk asset, or
   on idiosyncratic regulatory news? Has the correlation regime shifted?
8. **Global divergence** — Are ex-US central banks converging with or diverging
   from the Fed, and what does that do to the dollar and to carry?
9. **Fiscal** — What is the deficit path, how is issuance composition shifting,
   and at what point does supply drive the long end?
10. **Positioning** — Where is consensus most crowded, and what unwinds it?

---

## Calibration notes

Patterns in the brief's own errors, noticed over time. Are calls systematically
too confident? Too slow to update? Over-weighting the loudest story? Write them
down here — a bias you've named is one you can correct for, and this is the
section that turns a pile of daily notes into something that actually improves.

*Empty until there's enough history to see a pattern.*
