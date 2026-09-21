# Daily Brief — Execution Runbook

This is the operating procedure for the scheduled daily run. The Routine points
here; everything the run needs is in this repo.

---

## Step 0 — Orient

1. Select the working branch and pull the latest config and archive:
   ```bash
   cd /home/user/abgoel0309 && git fetch origin
   # Prefer the default branch once the workflow has been merged there;
   # fall back to the branch the files currently live on.
   if git cat-file -e origin/main:prompts/daily-brief.md 2>/dev/null; then
     git checkout main && git pull origin main
   else
     git checkout claude/daily-market-summary-6dbolq
     git pull origin claude/daily-market-summary-6dbolq
   fi
   ```
   Everything below reads from, and commits to, that branch.
2. Read `config/sources.md` — the source universe for this run.
3. Read `config/brief-template.md` — structure and the analysis standards.
4. Read `THEMES.md` — open theses and calls you are accountable to.
5. Read the two most recent files in `briefs/` so today builds on yesterday
   instead of restarting cold. Continuity is the point: a reader should be able
   to follow a theme across weeks.

Today's brief is `briefs/YYYY-MM-DD.md` using today's date in US Eastern.

---

## Step 0.5 — Preflight: can you deliver?

**Do this before spending the research budget.** A sweep costs several minutes;
discovering afterwards that the result can't be saved wastes all of it and, worse,
loses the analysis.

```bash
git push --dry-run origin "$(git branch --show-current)" 2>&1 | tail -5
```

Record the answer as `PUSH_OK = yes|no`. It decides how Step 5 delivers.

- `PUSH_OK = yes` — normal path: commit and push.
- `PUSH_OK = no` — the scheduled session lacks repo write credentials. **Do not
  abort.** Continue the full run and deliver through the fallback channels in
  Step 5. The analysis is the valuable part; the transport is incidental.

Either way, say which path you took in the final reply so a persistently broken
push surfaces on day one instead of quietly producing nothing for a week.

---

## Step 1 — Sweep

Budget roughly 25–40 searches and fetches. Work in parallel where possible —
batch independent searches in a single round rather than serializing them.

Sweep in this order, because later steps should be informed by earlier ones:

**A. Market state first.** Get the actual numbers before reading anyone's
opinion about them. Rates, equity indices, crypto, FX, commodities, vol. You
cannot evaluate commentary without knowing what the tape did.

**B. Official and primary.** Anything released since the last brief: data
prints, central bank communication, Treasury operations, regulatory filings,
rulemaking. Check release calendars for what landed overnight and this morning.

**C. Institutional and economist commentary.** How are serious people reading
the same facts? Note where they disagree with each other — disagreement among
informed observers is more interesting than consensus, and marks where the real
uncertainty sits.

**D. News.** Fill in what the primary sources don't cover: corporate events,
geopolitics, market-structure stories.

**E. Forums and sentiment.** Last, deliberately. Read these for the distribution
of belief and how fast it's shifting, after you already know what's true. Cover
equity, macro, rates, and crypto communities. Check prediction market odds.

**F. Gap check.** Before writing, ask what would embarrass you to have missed.
Chase that specifically. The failure mode of a sweep is confirming what you
already expected rather than finding what you didn't.

### Sweep discipline
- Capture the **link and the figure** as you go. Reconstructing citations after
  the fact is where fabrication creeps in.
- When two sources conflict, note it and prefer the primary. If both are
  primary, report the conflict — it's usually interesting.
- A source being unreachable is a log entry, not a reason to fill the gap
  from memory.

---

## Step 2 — Think before writing

Do not start drafting from the top. Before writing a word:

- What are the 3–5 things that genuinely matter today, ranked by effect on the
  forward path rather than by headline volume?
- What is the causal chain on each? If you can't state the transmission
  mechanism, you don't understand it well enough to write it.
- Where do the asset classes **disagree** with each other? That's section 7 and
  it's usually the most valuable part of the brief.
- What does today change about the open theses in `THEMES.md`?
- What is the strongest argument against your own read?

A brief that is just a reorganized news digest has failed. The value is in the
synthesis across asset classes and the honest accounting of what you got wrong.

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

## Step 4 — Update the running memory

1. Update `THEMES.md`:
   - Score open calls: tracking / broken / resolved, with the evidence.
   - Add new theses opened today, each with its falsifier and a confidence.
   - Retire theses that resolved. Keep them in the resolved log with the
     outcome — deleting your misses destroys the only mechanism that makes this
     archive worth keeping.
2. Copy today's brief to `briefs/latest.md` so there's a stable path to read.

---

## Step 5 — Deliver

Delivery is layered so the brief survives any single channel failing. Work
through every layer that applies.

### Layer 1 — Always: publish as an Artifact

Regardless of `PUSH_OK`, publish the brief as an Artifact. It needs no git
credentials, produces a stable link, and reads well on a phone — which is where
a 6am brief actually gets read.

Write the brief to an HTML page and publish it with the Artifact tool, titled
`Market Brief <Month D, YYYY>`. Keep it clean and readable: clear section
headings, the twelve sections in order, generous line height, working in both
light and dark mode. Load the `artifact-design` skill before writing the page.

Record the returned URL — it goes in the reply.

### Layer 2 — If `PUSH_OK = yes`: commit to the repo

```bash
git add briefs/ THEMES.md
git commit -m "Daily brief: YYYY-MM-DD"
git push origin "$(git branch --show-current)"
```

Push to the branch selected in Step 0 — never a different one. If the push
races with another commit, `git pull --rebase` and retry. On network failure,
retry up to 4 times with exponential backoff (2s, 4s, 8s, 16s).

If the push fails here despite the preflight passing, fall through to Layer 3
rather than discarding the work.

### Layer 3 — If `PUSH_OK = no`: put the full brief in the reply

The reply is delivered by push and email notification, so it is the channel of
last resort that always works. Include the **complete brief text**, not a
summary — an un-saved brief that exists only as a summary is a lost brief.

Also state plainly, at the top of the reply, that the repo commit failed and
that `THEMES.md` could not be updated, so the next run starts without today's
scorecard. That degradation is worth flagging every time it happens: it is the
thing that quietly breaks the memory the whole system depends on.

## Step 6 — Report back

Reply with a short, phone-readable summary:

- The one-liner
- The top 2–3 items
- The Artifact link
- The committed file path, if Layer 2 succeeded
- **Which delivery path was used**, and any degradation (push failed, sources
  unreachable, thin coverage)

Keep it short. The full brief lives in the Artifact and the repo for whoever
wants the detail — the reply is the headline, not the document.

---

## Failure handling

**Partial sweep** (some sources unreachable): write the brief anyway with the
coverage log stating exactly what's missing. A brief with honest gaps beats no
brief.

**Thin news day**: write a short brief. Two sections saying "quiet, here's the
trend it sits inside" is a correct and useful output. Never pad to hit a length
target — padding trains the reader to skim, which destroys the brief's value on
the days that matter.

**Can't verify a market level**: mark it `[unverified]` and move on. Do not
substitute a remembered figure.

**Push rejected or unavailable**: pull with rebase and retry once. If it still
fails, deliver via Layer 1 and Layer 3 and say so explicitly. Never end a run
having produced analysis that reached nobody.
