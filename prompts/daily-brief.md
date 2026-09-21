# Daily Brief — Execution Runbook

Operating procedure for the scheduled daily run. The Routine points here;
everything the run needs is in this repo.

---

## Important: scheduled runs have READ-ONLY repo access

This was verified, not assumed. A trigger-fired session:

- has **no checkout** at `/home/user/abgoel0309` — it must clone
- **can read** the repo (public read over HTTPS works)
- **cannot push** — the remote rejects it with `403: repo not in session's
  authorized set`, because triggers carry no attached repo source

So the repo is the **input** (runbook, config, seed themes) and an **Artifact**
is the **output**. Do not waste a run trying to defeat the 403 — it is a
platform authorization boundary, not a misconfiguration.

Continuity is preserved by reading the previous day's published brief, not by
reading a committed file. Details in Step 0 and Step 4.

---

## Step 0 — Orient

```bash
cd /tmp && rm -rf brief-repo
git clone --depth 20 https://github.com/abgoel0309/abgoel0309 brief-repo
cd brief-repo
# Use main once the workflow is merged there; otherwise the feature branch.
git checkout main 2>/dev/null || git checkout claude/daily-market-summary-6dbolq
```

Then read, in order:

1. `config/sources.md` — the source universe for this run.
2. `config/brief-template.md` — structure and the analysis standards.
3. `THEMES.md` — the seed standing questions and any hand-edited theses.

Then recover the running memory:

4. List your recent artifacts (`Artifact` tool, `action: "list"`) and find the
   most recent titled `Market Brief …`. Read it. Carry forward its section 11
   scorecard and its open theses — that is what makes today build on yesterday
   rather than restarting cold.

If no prior brief exists, this is the first run: start the thesis log from the
standing questions in `THEMES.md`.

Today's date is today's date in **US Eastern**. Get the generated timestamp
from `date`, don't estimate it — a brief whose own header time is wrong
undermines every other figure in it.

---

## Step 1 — Sweep

Budget roughly 25–40 searches and fetches. Batch independent searches in a
single round rather than serializing them.

Sweep in this order, because later steps should be informed by earlier ones:

**A. Market state first.** Get the actual numbers before reading anyone's
opinion about them. Rates, equity indices, crypto, FX, commodities, vol. You
cannot evaluate commentary without knowing what the tape did.

**B. Official and primary.** Anything released since the last brief: data
prints, central bank communication, Treasury operations, regulatory filings,
rulemaking. Check release calendars for what landed overnight and this morning.

**C. Institutional and economist commentary.** How are serious people reading
the same facts? Note where they disagree with each other — disagreement among
informed observers marks where the real uncertainty sits.

**D. News.** Fill in what primary sources don't cover: corporate events,
geopolitics, market-structure stories.

**E. Forums and sentiment.** Last, deliberately. Read these for the distribution
of belief and how fast it's shifting, after you already know what's true. Cover
equity, macro, rates, and crypto communities. Check prediction market odds.

**F. Gap check.** Before writing, ask what would embarrass you to have missed,
and chase that specifically. The failure mode of a sweep is confirming what you
already expected rather than finding what you didn't.

### Sweep mechanics — read this before searching

**`WebSearch` is your only research channel.** This environment's network
policy blocks general outbound HTTPS: `curl` and `WebFetch` both fail with 403 /
`EGRESS_BLOCKED` on every research domain, government sources included. Do not
burn calls rediscovering this.

Recover most of the lost precision with `allowed_domains`, which scopes a search
to primary sources and is verified working:

```
WebSearch(query: "...", allowed_domains: ["federalreserve.gov", "stlouisfed.org"])
```

`config/sources.md` lists the domain sets to scope by. Use them for anything you
would otherwise want to read off a release page.

### Sweep discipline
- Capture the **link and the figure** as you go. Reconstructing citations after
  the fact is where fabrication creeps in.
- **Cross-verify every headline number against two independent sources.** Search
  indexes go stale and confuse release dates. This is not optional caution: it
  is the check that stops a wrong CPI print going out with full confidence.
- When sources conflict, **report the conflict and the range**. Never silently
  pick one. Log it in section 12.
- A figure from a search snippet is Tier 1 only if the search was scoped to the
  primary domain *and* it is corroborated. Otherwise it's Tier 2 — attributed
  reporting, labeled as such.
- Discard sources that read as content-farm or auto-generated output, even when
  they carry the precise number you want. A plausible-looking fabricated level
  is worse than `[unverified]`.
- A source being unreachable is a log entry, not licence to fill the gap from
  memory.

---

## Step 2 — Think before writing

Do not draft from the top. First:

- What are the 3–5 things that genuinely matter today, ranked by effect on the
  forward path rather than by headline volume?
- What is the causal chain on each? If you can't state the transmission
  mechanism, you don't understand it well enough to write it.
- Where do the asset classes **disagree**? That's section 7, usually the most
  valuable part of the brief.
- What does today change about the open theses carried forward from Step 0?
- What is the strongest argument against your own read?

A brief that is a reorganized news digest has failed. The value is the synthesis
across asset classes and the honest accounting of what you got wrong.

---

## Step 3 — Write

Follow `config/brief-template.md` exactly — all twelve sections, in order.
Hold to the analysis standards in Part 1 of that file, especially:

- No fabricated numbers. Ever. `[unverified]` is always available.
- Observation and interpretation kept structurally separate.
- Confidence level and a concrete falsifier on every non-trivial call.
- Tier 3 sentiment labeled as belief, never promoted to fact.
- Gaps and thin coverage logged honestly.
- Commit to views. Hedged mush is not rigor.

---

## Step 4 — Carry the memory forward

Because `THEMES.md` cannot be written back to the repo, **the brief itself is
the memory**. Section 11 must therefore be self-contained enough for tomorrow's
run to pick up cold from it:

- Score every open thesis: tracking / strained / broken / resolved, with the
  evidence that moved it.
- Restate each still-open thesis in full — claim, mechanism, falsifier,
  confidence. Do not write "T-003 unchanged"; tomorrow's run has no other copy.
- Keep resolved theses with their outcome, including the ones that were wrong.
  Deleting misses destroys the only feedback loop this system has.

---

## Step 5 — Deliver

### Primary: publish as an Artifact

This is the channel that works. Load the `artifact-design` skill, then write the
brief as an HTML page and publish it with the `Artifact` tool.

- **Title:** `Market Brief <Month D, YYYY>` — the `Market Brief` prefix is load-
  bearing, since Step 0 of the next run finds it by that name.
- Twelve sections in order, clear headings, comfortable line height, working in
  both light and dark mode, readable at phone width.
- `icon: "chart"`.

Record the returned URL for the reply.

### Secondary: the reply

The reply is delivered by push and email notification, so it always lands. See
Step 6.

### Do not attempt a repo push

It will fail with a 403 and waste the run's remaining budget. If repo archiving
is wanted, the owner has to either create the Routine from the claude.ai
Routines UI with the repository attached, or copy the published brief into the
repo by hand.

---

## Step 6 — Report back

Reply with a short, phone-readable summary:

- The one-liner
- The top 2–3 items
- The Artifact link
- Any degradation: unreachable sources, thin coverage, a failed sweep area

Keep it short. The full brief is in the Artifact — the reply is the headline,
not the document.

---

## Failure handling

**Partial sweep** (some sources unreachable): write the brief anyway, with the
coverage log stating exactly what's missing. A brief with honest gaps beats no
brief.

**Thin news day**: write a short brief. Two sections saying "quiet, here's the
trend it sits inside" is a correct and useful output. Never pad to hit a length
target — padding trains the reader to skim, which destroys the brief's value on
the days that matter.

**Can't verify a market level**: mark it `[unverified]` and move on. Never
substitute a remembered figure.

**Artifact publish fails**: put the **complete brief text** in the reply, not a
summary. An un-saved brief that exists only as a summary is a lost brief.

**Clone fails**: the repo is public; retry once. If it still fails, run from the
instructions in the Routine prompt alone and note the degraded coverage — you
will be missing the full source list, so say so.
