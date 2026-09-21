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

## Step 5 — Commit and push

```bash
git add briefs/ THEMES.md
git commit -m "Daily brief: YYYY-MM-DD"
git push -u origin "$(git branch --show-current)"
```

Push to the branch selected in Step 0 — never to a different one. If the push
races with another commit, `git pull --rebase` and retry. On network failure,
retry up to 4 times with exponential backoff (2s, 4s, 8s, 16s).

---

## Step 6 — Report back

Reply with a short summary: the one-liner, the top 2–3 items, and the link to
the committed file. Keep it to something readable on a phone — the full brief
is in the repo for whoever wants the detail.

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

**Push rejected**: pull with rebase and retry. If it still fails, report the
failure with the brief's content inline so the analysis isn't lost.
