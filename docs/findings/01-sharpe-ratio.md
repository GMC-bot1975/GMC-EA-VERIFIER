# 01 — THE STRATEGY TESTER'S SHARPE RATIO IS NOT WHAT MOST PEOPLE THINK

**And my first version of this document was wrong.** That is part of the finding, so it stays in.

---

## What I thought

My backtest report said:

```
Sharpe Ratio : 4.57
Total Trades : 385
Period       : 2025.01.01 - 2026.08.21   (1.64 years, ~235 trades/year)
```

A Sharpe of 4.57 would make me one of the best traders alive, and I am definitively not. So I
exported the deals and computed it from the trade results:

```
mean / stdev of per-POSITION results   =  0.173
annualised: 0.173 × √235               =  2.65
```

I concluded the report's field was a **per-trade** figure, not comparable to a published Sharpe,
and I wrote that into a draft article and a video script.

**It is not true.**

---

## What it actually is

From the MQL5 forum, answered by a moderator, and from the platform's own documentation and a
published implementation article:

> "The Strategy Tester calculates the Sharpe ratio using **logarithmic returns of equity per bar
> (last tick), ignoring bars with no change**, and with Rf=0."

Then annualised by **√(bars in a year ÷ bars where equity changed)**.

The calculation was revised in platform build 3210 "to match the traditional formula, in which the
value corresponds to a one-year interval", and to include equity movement against **open**
positions, which the previous version ignored.

| step | what |
|---|---|
| series | **equity at each bar's last tick** — not trade results |
| transform | log returns, `ln(E_t / E_t-1)` |
| filter | **bars where equity did not change are DROPPED** |
| risk-free | 0 |
| ratio | mean ÷ sample standard deviation |
| annualisation | **× √(bars in a year ÷ bars with equity changes)** |

**So 4.57 is a genuine annualised Sharpe ratio.** Not per-trade, not a bug. It answers a different
question from 2.65.

### Both are legitimately "Sharpe"

| | per-position (2.65) | the report's field (4.57) |
|---|---|---|
| series | net result of each **position** | **log return of equity per bar** |
| sees drawdown *inside* a trade? | **no — blind to it** | **yes** |
| idle periods | not applicable | **dropped before annualising** |

### The thing I could not explain, explained

My draft said: *"4.57 ÷ 0.173 = 26.4, which matches neither √385 nor √235."*

Of course it doesn't. **26.4 was never going to be a function of the trade count**, because the
denominator series is *bars*, not trades. I was hunting the wrong ratio.

---

## THE MEASUREMENT WORTH TAKING AWAY

Same EA, same symbol, same timeframe. **Two different windows:**

| window | years | positions | per-POSITION | the report's field | gap |
|---|---|---|---|---|---|
| short | 1.64 | 385 | **2.65** | **4.57** | **+1.92** |
| long | 7.64 | 1,858 | **1.31** | **1.19** | **−0.12** |

**The gap does not merely shrink on the longer window — it reverses.**

So *"the report reads high"* is **not a rule.** It read 1.92 high on one window and 0.12 low on the
other, with the same EA on the same instrument.

This is exactly what the moderator said would happen: the equity-based method converges with the
trade-based one only over long periods, and short windows show large differences from sampling and
time-scale effects. **Measured here on my own two reports.**

**The practical lesson for a short backtest: a 1.64-year report can show a 1.92 Sharpe discrepancy
purely from which series you measure.** That is a much more useful warning than anything in my
original draft.

---

## The genuine quirk, which nobody discusses

**Idle bars are dropped, and then the result is annualised by the ratio of bars.**

A fund annualises over **every** period, including the flat ones. The Strategy Tester annualises
over only the bars where equity moved. So **a strategy that trades rarely receives a larger
multiplier than a fund with identical economics would.**

That is documented, it is arithmetic, and it means the field is still not directly comparable to a
Sharpe quoted on a fund factsheet — which was my original instinct, arrived at for entirely the
wrong reason.

---

## Check your own, three ways

1. **Per-position:** net result of each position (profit + swap + commission, grouped by
   **position**, not by deal), mean ÷ standard deviation, × √(positions per year).
2. **Per-bar over ALL bars:** log returns of the equity curve, annualised over every bar. **This is
   the fund-comparable one.**
3. **Per-bar, dropping flat bars, annualised by the ratio.** This should reproduce the report.

**The gap between 2 and 3 is caused by nothing but the dropped bars.**

> **One trap in step 1:** group by **position**, not by deal. A partial close writes *two deals for
> one position*, and counting deals inflates your trade count and your win rate at the same time.
> This project's own daily report was doing exactly that — 59 closing deals against 47 real
> positions, a **25% overstatement** of sample size, under a label that said "trades".

---

## What this cost, and what it bought

**Cost:** one draft article and one video script, both stopped before publication.

**Bought:** a sourced claim instead of a guess, a measurement nobody had published, and a tool that
no longer tells people their Sharpe is "wrong" when it is simply computed on another series.

The check in the tool now reports **both figures, labelled by series**, and states that a divergence
is **information, not an error**.
