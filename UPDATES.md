# PROJECT UPDATES

Newest first. Dates are the date the thing happened, not the date it was written up.

The rule for this file: **record what was measured, including the times it contradicted something I
had already said.** An update log that only contains progress is marketing.

---

## 2026-10 — Forward test running on a third-party EA

A dedicated terminal and a dedicated demo account now carry **one publicly available EA, written by
somebody else**, under a hash-locked registration written before it opened a single position.

- **Runs to 300 closed trades or 8 weeks**, whichever comes first.
- **Hard kill condition declared in advance:** equity down 17%, which is 1.5× the EA's own
  backtested maximum drawdown. Not a number chosen later.
- **The account is dedicated and had zero deals in its entire history**, so every trade on it is
  attributable with no ambiguity.
- **Nothing goes on and nothing comes off** until the endpoint. Not even to read the EA's settings —
  doing so would require a restart, and that would be a change.

**The EA is not named here, and will not be until its author has been contacted and given a right of
reply.** Publishing adverse findings about someone's work before they have had a chance to answer is
not verification, it is just noise with numbers attached.

Crash safety was proven by demonstration before the freeze, not assumed: the EA re-attaches itself
after a terminal restart, and has since survived an unplanned power loss unaided with 2h 38m of
downtime.

---

## 2026-10 — Eleven checks audited; nine defects found in my own tool

A line-by-line audit of the verifier against itself. **Seven fixed, two still open.** The ones worth
publishing:

- **A check compared two figures computed on different bases** and could have told someone their
  edge had collapsed out of sample when nothing was wrong. Both sides now computed identically, and
  a cross-basis comparison is reported but **not permitted to fail an EA**.
- **A constant measured on gold was being applied to every instrument** — in the check whose output
  is the most useful single number the tool produces. Now derived from the symbol.
- **Fixing that exposed a units mislabel**: the constant was the value of one *unit of price*, not
  one *point*, and everything had been calling it "points" for months. The arithmetic was
  self-consistent and the word was wrong — **two errors cancelling, which became a 100× error the
  moment one was corrected.**
- **A stored benchmark ratio about gold** was being shown to strategies on unrelated symbols, while
  the tool was already computing the correct benchmark for the actual symbol and ignoring it.
- **A losing EA passed the concentration check**, because a negative total flipped a sign and
  cleared both thresholds.
- **The documentation promised a cost-margin rule the check had never implemented.** Its condition
  was `abs(total) >= 0.0` — true for every real number — and the result was then never used.
  Documentation corrected rather than a rule invented, because the rule needs a per-symbol cost
  figure and asserting one would be the same class of error.

See [docs/what-it-checks.md](docs/what-it-checks.md) for what each check does now.

---

## 2026-10 — A published claim retracted before publication

I had drafted an article and a video script stating that MetaTrader's Strategy Tester Sharpe field
is a per-trade figure and not comparable to a published Sharpe ratio.

**That was false.** The answer was publicly documented and I had not looked hard enough. Both drafts
were stopped before release.

What replaced it is better, and measured: the field is a **per-bar log equity** Sharpe with idle
bars dropped, and the gap between it and a per-trade calculation **reverses sign** depending on
window length — +1.92 on 1.64 years, −0.12 on 7.64 years, same EA.

Full write-up: [docs/findings/01-sharpe-ratio.md](docs/findings/01-sharpe-ratio.md).

---

## 2026-10 — Daily report was counting deals as trades

Two independent counts of the same account disagreed. The ledger said 47 closed trades; the daily
report said 56.

Three separate causes, and **only two were errors**:

1. The snapshots were 15 hours apart and three trades closed in between. **Not an error.**
2. The report counted **closing deals** and labelled them "trades". A partial close writes two deals
   for one position: **59 deals against 47 positions, a 25% overstatement of sample size.**
3. Its net omitted **entry commission**, which is charged on the opening deal.

The ledger was right, and right for a good reason: it grouped by position *and* it reconciled to
balance minus deposits.

**This is the error I warn other people about in finding 01, found in my own daily report.**

---

## 2026-09 — The research record

**45 strategies built. 56 pre-registered tests. Not one passed.**

Every bar hash-locked before the test ran. No false positive has ever been deployed, which is the
only claim here worth anything.
