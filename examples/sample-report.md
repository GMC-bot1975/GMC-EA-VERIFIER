# SAMPLE REPORT — ONE OF MY OWN EAs

**This is real output on a real backtest, not a mock-up.** The EA is mine, the account is my own
demo, and **it failed.**

I am using one of my own rather than a client's or a stranger's, for the obvious reason: I can
publish every number without asking anyone's permission, and nobody's work is being criticised but
my own.

---

## The input

A gold breakout EA on H1, with an out-of-sample report alongside the in-sample one, and **164
declared trials** — the honest project-wide count of every configuration ever tried here, not the
four tried on this particular idea.

```
EA        : GMC_BreakL1_v2
symbol/TF : XAUUSD  H1 (2025.01.01 - 2026.08.21)
deposit   : 25 000.00 GBP   net 8 799.73   PF 1.67   trades 385
drawdown  : 2 008.70 (5.74%)
```

On the face of it: **+35% in 1.64 years, profit factor 1.67, under 6% drawdown.** Most people would
call that a good EA.

---

## The two Sharpe figures

```
Sharpe    : 2.65  PER-POSITION, annualised (385 positions at 235/yr)
            4.57  the tester's field - PER-BAR log equity returns, annualised,
                  idle bars dropped
            BOTH ARE REAL SHARPE RATIOS ON DIFFERENT SERIES. Neither is wrong.
            GAP +1.92. This is INFORMATION, NOT AN ERROR.
```

See [finding 01](../docs/findings/01-sharpe-ratio.md) for why these differ and why the gap
**reverses** on a longer window.

---

## The verdict

```
[OK  ] V1 data      History Quality 99% - but note HQ measures GAPS and TICK DENSITY only.
                    It is BLIND to a mislabelled timeframe.
[OK  ] V1b bars     median 3600s matches H1; 96% of gaps exactly nominal; commonest
                    NON-nominal gap 7200s at 3%; bar density first-half vs second 1.00x
[OK  ] V2 sample    385 trades
[OK  ] V2 provable  t = 2.65 x sqrt(1.64) = 3.39  (against the declared bar).
                    1.64 years CANNOT prove a Sharpe below 1.17

  OOS : net 2 760.50   PF 1.29   Sharpe 1.70   trades 257

[WARN] V3 IS/OOS    Sharpe 2.65 -> 1.70 (both per-position), gap +0.95
[FAIL] V4 benchmark strategy +35.2% vs BUY AND HOLD +67.3%  - HOLDING THE INSTRUMENT
                    BEAT IT. An investor asks this first and there is no answer to it
[FAIL] V5 trials    p = 0.0004; corrected for 164 declared trial(s) -> 0.0641
                    - does not survive the trial count
[OK  ] V6 drawdown  CAGR 20.21%/yr against max DD 5.7% -> Calmar 3.52
[OK  ] V7 concentr  top 5 of 385 trades = 34% of all profit
[WARN] V8 slippage  BREAKEVEN SLIPPAGE 3.622 OF PRICE per trade. At 5.000 of price
                    the net goes 8799.73 -> -3347.71  - thin
[OK  ] V9 margin    net 8799.73 on turnover 35064.33 = 25.096% per unit traded
[OK  ] V10 size     lots used 0.01 .. 0.21

  2 FAIL   2 WARN   8 OK
  VERDICT: NOT VIABLE as presented. 2 disqualifying finding(s) above.
```

---

## What actually killed it

### V4 — buy-and-hold beat it, and it is not close

**+35.2% for the strategy against +67.3% for doing nothing.**

The benchmark is computed automatically from the report's own symbol and window, with financing
**derived live from the broker** — in this run, 4.997%/yr from `swap_long -56.282 points/night`,
rather than a stored guess. See [finding 03](../docs/findings/03-financing-carry.md) for why that
derivation matters.

Gold rose hard over this window. The EA was long gold. **It captured about half the move while
taking on execution risk, overnight financing and model risk to do it.**

This is the check people most want to skip, and it is the first thing anyone with money will ask.

### V5 — it does not survive the trial count

**p = 0.0004 looks excellent.** Corrected for 164 declared trials it becomes **0.0641**, which does
not clear 0.05.

That correction is the honest one. 164 is every configuration this project has ever tried, and
roughly five in a hundred of them would clear a 5% bar by luck alone. **Had I declared only the
four trials on this specific idea, it would have passed** — which is exactly why the trial count is
declared in advance and not chosen afterwards.

---

## And what the warnings are telling me

- **V3** — the edge weakened out of sample, 2.65 to 1.70. Not a collapse, but it went the wrong
  way on the half it was not fitted to.
- **V8** — **3.6 of price of adverse fill per trade erases the entire result.** Typical spread on
  this instrument is 0.20–0.30, so there is room. But a stop here was once filled **61.50 of price
  past** where it sat on a news day. One of those is worth about seventeen trades' worth of edge.

---

## The point of showing you a failure

**This EA was live. I built it, I believed in it, and the report above is why I should not have.**

Two checks out of eleven killed it, and **neither of them is about the equity curve.** The curve was
fine. What failed was the comparison against doing nothing, and the honest accounting of how many
times I had already rolled the dice.

**A verification tool that passes things is not doing its job. Nor is one that fails everything** —
so for completeness: the finding this project most wants, and has not yet had, is an EA that clears
all eleven. Around 164 trials, 0 passes. If yours is the first, that result gets published as
loudly as this one.

---

# CHECK MY ARITHMETIC

**Every figure above that can be derived from the others is listed here with its formula, so you
can check it rather than trust it.** If one of these does not reproduce for you, that is an
[issue](../CONTRIBUTING.md) worth opening.

Starting values from the report header: **deposit 25,000.00**, **net 8,799.73**, **385 positions**,
**max equity drawdown 5.74%**, window **2025.01.01 – 2026.08.21**, **32.06 total lots traded**.

| figure | formula | works out to | report prints |
|---|---|---|---|
| years | window length ÷ 365.25 | 1.6345 | **1.64** |
| **t** | `Sharpe × √years` = 2.65 × √1.6345 | 3.3880 | **3.39** |
| **CAGR** | `((1 + net/deposit)^(1/years) − 1) × 100` | 20.26% | **20.21%** |
| **Calmar** | `CAGR ÷ max drawdown %` = 20.21 ÷ 5.74 | 3.5209 | **3.52** |
| **breakeven slippage** | `net ÷ (total lots × value of 1.0 of price per lot)` = 8,799.73 ÷ (32.06 × 75.779) | 3.6221 | **3.622** |
| strategy return | `net ÷ deposit` = 8,799.73 ÷ 25,000 | +35.20% | **+35.2%** |

### ⚠ The one that looks wrong and is not

**V5 prints `p = 0.0004` and then `-> 0.0641` after correcting for 164 trials.**

`0.0004 × 164 = 0.0656`, **not 0.0641.** It looks like an arithmetic error.

It is a **display rounding** artefact. The true one-sided p from a t-distribution with 384 degrees
of freedom at t = 3.388 is **0.000389**, and `0.000389 × 164 = 0.06372`. The report rounds p to four
decimal places for display but **corrects using the full value**, which is the right way round —
correcting a rounded number would be the actual error.

This is flagged here because it is exactly the kind of thing a careful reader spots and concludes is
sloppiness. **If you find something that looks like this, say so** — I would rather explain a
rounding convention than have someone quietly decide the numbers are unreliable.

### What you cannot reproduce from this page, and why

Being straight about the limits of this example:

| figure | why it is not reproducible from here |
|---|---|
| the **per-position Sharpe of 2.65** | needs the deal export, position by position |
| the **buy-and-hold +67.3%** | computed from my broker's own gold history and its live `swap_long` (4.997%/yr, derived at run time). Another broker's feed and swap rate will give a different figure |
| the **tester's 4.57** | comes from the Strategy Tester report itself |

**✅ THE RAW REPORTS ARE NOW PUBLISHED, so all three are reproducible after all:**

- **[be10new_IS_report.htm](be10new_IS_report.htm)** — the in-sample run, 2025.01.01 – 2026.08.21
- **[be10new_OOS_report.htm](be10new_OOS_report.htm)** — the out-of-sample run

**Download them and every figure on this page is checkable end to end**, including the
per-position Sharpe (export the deals and group by position, not by deal) and the tester's own 4.57.

They were scanned before publishing: **no account number, no email address, no credential** — only
the demo server name. They **do** contain the EA's full input list, i.e. my strategy's settings.
That is published deliberately: **the EA failed its own pre-registered bar, so the settings are not
worth protecting, and being auditable is worth more than hiding a losing configuration.**

**The one figure still not reproducible from these files alone is the buy-and-hold +67.3%**, because
it is computed from my broker's own gold history and its live `swap_long` (4.997%/yr, derived at run
time). Another broker's feed and swap rate will give a different number — which is the point of
deriving it rather than storing it. See
[finding 03](../docs/findings/03-financing-carry.md).
