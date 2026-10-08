# THE ELEVEN CHECKS

**Every one of these was written because this project got it wrong first.** The scars are named so
nobody has to relearn them.

They are described here in enough detail to reproduce by hand. That is deliberate.

---

## V1 — Data quality

Is the history real, or did the tester invent it?

Below 90% history quality the tester filled the gaps itself, and those fills are not the broker's
prices.

**But a pass here means less than it looks.** History Quality measures **gaps and tick density
only**. It is completely blind to whether a bar is the timeframe it claims to be — it reported
**99%** on eighteen years of daily bars sitting inside an H4 series. That caveat now travels with
the pass, because "V1 OK" was being read as broader assurance than it is.

## V1b — Mislabelled timeframe

**The one that catches a broker serving you the wrong bars.**

Measure the actual gap between bars and compare it to what the timeframe claims. Three signals,
because no single statistic sees every shape of this:

| signal | catches |
|---|---|
| **median gap** vs nominal | a wholly mislabelled series |
| **share of gaps at nominal** (fail below 80%) | **a minority of wrong bars, which the median cannot see** |
| **bar density, first half of the window vs second** (fail at 2× or worse) | a regime change in the data — and it assumes nothing about trading calendars, because it compares the data only against itself |

The second and third exist because of a real defect in this tool. It originally used the **median
gap alone**, and a median is a *majority* statistic. A series that is 77% correct H4 and 23% daily
bars has a perfect median — so the check passed. Reproduced: **15,450 correct gaps plus 4,680 daily
gaps gives a median of exactly nominal, while 23% of the history is the wrong bar size.** The
share-at-nominal figure was being computed and printed the whole time, and never acted on.

## V2 — Sample and provability

Two parts.

**Sample:** below 100 trades, it fails. That threshold is a **house rule, not a law** — 60 trades
at a per-trade Sharpe of 0.5 gives t = 3.87, which is comfortably significant. So the check now
prints the per-trade Sharpe that *would* clear the bar on your sample, rather than dismissing a
genuine low-frequency strategy.

**Provability:** `t = Sharpe × √years`, against a bar of 1.5, abandon below 1.0. It also tells you
the Sharpe your window is incapable of proving, whatever you do.

## V3 — In-sample vs out-of-sample

The half you did not fit to is the only half that counts.

Without a split, an overfit is invisible: on one 47-configuration sweep here, the in-sample winner
**lost half its edge** out of sample. It would have been the one deployed.

**Compare like with like.** This check briefly compared an in-sample figure computed one way against
an out-of-sample figure computed a different way. The gap was meaningless, and because the two can
diverge substantially on short windows it could have told someone their edge had collapsed when
nothing was wrong. Both sides are now computed identically, and if either has to fall back to a
different basis the result is reported but **not allowed to fail the EA**.

## V4 — The benchmark

**The one that kills most EAs, and the one people most want to skip.**

Did it beat simply buying the instrument and sitting on it?

It is computed automatically from the report's own symbol and window **so it cannot be skipped**,
with financing derived live from the broker per symbol rather than hardcoded.

An investor asks this first, and if the answer is no there is no answer to it. Around 164 trials
here, and the survivor still lost to buy-and-hold.

## V5 — Multiplicity

47 configurations means about two clear a 5% bar by luck. Significance is corrected for the
declared trial count.

If only one trial is declared, the report says plainly that the number is wrong if more were tried
and discarded.

## V6 — Drawdown against return

Return over maximum drawdown, compared against buy-and-hold on **the same symbol over the same
window** — not against a stored constant.

**And the thing people get wrong: you cannot fix this by sizing up.** Leverage multiplies return
and drawdown together, so the ratio is flat in position size. If the shape is bad, it is bad at
every size.

## V7 — Concentration

Sort the trades by result. How much of the profit is in the top five?

One EA here had **five positions out of 267 accounting for 79% of all profit.** Remove them and
there is no edge. It takes ten seconds in a spreadsheet and it is sobering.

A negative total is now failed explicitly — a losing EA used to flip the sign and pass this check
clean.

## V8 — Slippage, and breakeven slippage

**The most useful single number this tool produces.**

Rather than passing or failing at an arbitrary figure, it answers: **how much adverse fill per
trade erases the entire result?**

That converts an abstract fragility into one quantity you can hold against the fills you actually
get. A strategy whose whole edge dies at a fraction of a typical spread is not a strategy.

The value of one unit of price is **derived from the symbol**, not stored — it was previously a
constant measured on one instrument and applied to all of them.

Why it matters: a stop here was filled **61.50 of price past where it sat** on a news day, turning
a modest loss into one about four times the size. **No backtest models that.**

## V9 — Margin per unit traded

Net result over total turnover.

**This is not a cost test, and it does not apply any cost-margin rule.** Said plainly because the
documentation used to claim it did, while the check itself reduced to "did it make money". V8 is the
cost-fragility check.

## V10 — Size feasibility

Did the position size sit on a ceiling, or on the broker's minimum?

If it did, **the test measured the limit rather than the strategy** — and nothing in the log says
so while it happens.

## V11 — Plausibility

Numbers that cannot happen mean an error, not genius.

A 2R target cannot produce a 5R average win. A profit factor above 3, or a win rate above 90%, is
flagged for a look at look-ahead, a mislabelled timeframe, or fills at prices that never traded.

These are warnings, not failures — occasionally they are real.

---

## WHAT A CLEAN VERDICT MEANS

**"Not yet disqualified."** Nothing more.

**No backtest can prove an EA works.** This tool exists to fail things cheaply, before they cost
money. The live forward record is the only evidence there is.
