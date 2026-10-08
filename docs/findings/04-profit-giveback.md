# 04 — WINNERS GAVE BACK 61% OF THEIR PEAK PROFIT

And no Sharpe ratio computed from trade results can see it.

---

## The measurement

33 closed positions, live demo, measuring each winning position's **peak unrealised profit** against
what it actually closed for:

```
average PEAK      profit :  +140.42
average REALISED  profit :   +54.28

give-back                :  61%
```

**Nearly two-thirds of the profit these trades reached was handed back before they closed.**

---

## Why this is invisible in the usual statistics

A per-trade Sharpe ratio, a profit factor, a win rate and an average win are all computed from
**closed results**. They see `+54.28`. They have no idea `+140.42` ever happened.

So a strategy can look stable and modestly profitable on every standard metric while systematically
converting large wins into small ones.

**This is the one thing the Strategy Tester's own Sharpe field does better than a per-trade
calculation** — being computed on per-bar equity, it at least sees the equity swing inside an open
position. See [finding 01](01-sharpe-ratio.md).

---

## How to measure it on your own trades

For each closed winning position you need the **maximum favourable excursion** — the best price the
position reached while open, not the price it closed at.

Two routes:

1. **From the tester:** run with every tick and log the position's unrealised profit on each tick,
   keeping the maximum per position ticket.
2. **From live or demo history:** for each position, pull the bars between open and close and take
   the extreme in your favour, then value it at your position size.

Then:

```
give-back % = 1 - (mean realised / mean peak)
```

**Do it for winners only.** Losers' excursions are a different question (they tell you whether your
stop is in the right place, which is worth knowing separately).

---

## What it does not tell you

**Nothing in this measurement says "exit earlier".**

The give-back figure is a description, not a prescription, and it is easy to draw the wrong
conclusion from it. In this project's own case, two trades peaking around +280 paid for everything
else — and any exit rule tight enough to capture most of the 61% would have cut both of them short.

**That is the real tension and the number alone does not resolve it:**

| tighten the exit | and you |
|---|---|
| bank more of the average peak | clip the rare large winner that pays for the strategy |

Which is why the change made here was **a partial close plus a WIDER trail**, not a tighter one —
bank half irrevocably, then give the remainder room. Whether that actually moved the 61% is an open
question, deliberately left open: it needs enough closed trades under the new rule to measure, and
re-measuring it early would be the exact thing [the methodology](../methodology.md) forbids.

**A strategy with 61% give-back is not necessarily broken. But you should know the figure, because
it is a fact about your trading that none of the standard metrics will ever show you.**
