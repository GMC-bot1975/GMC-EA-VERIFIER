# 02 — A BROKER SERVED ME DAILY BARS INSIDE AN H4 CHART FOR EIGHTEEN YEARS

**History Quality reported 99% the whole time**, because it does not measure this.

---

## The measurement

Gold, H4, a long backtest reaching back to the late 1990s:

```
bars per year BEFORE 2016   :  ~260
bars per year AFTER  2016   :  ~1,545
bars exactly 24 hours apart :  3,819
```

A genuine H4 series on a 24/5 instrument gives roughly **1,500 bars a year**. About **260** is a
*daily* series.

So for the first eighteen years of that test, **the chart said H4 and the data was daily.**

---

## Why it matters more than it sounds

Every lookback in the EA silently changed meaning.

**A 20-bar lookback meant 20 DAYS instead of 80 hours** — a window roughly six times longer than
the strategy was written for, for most of the test's history.

The strategy being tested was not the strategy. And nothing anywhere said so.

---

## Why History Quality did not catch it

**History Quality measures gaps and tick density.** It answers *"is this series continuous and
densely populated?"*

Daily bars are perfectly continuous and perfectly dense. They are just **the wrong size**.

It reported **99%**.

This is the single most useful thing in this document: **a high History Quality figure is not a
statement that your data is the timeframe it claims to be.** It was never measuring that.

---

## How to check yours — one minute

Count bars per year and compare to the nominal rate:

| timeframe | expected bars/year, 24/5 instrument |
|---|---|
| D1 | ~260 |
| H4 | ~1,545 |
| H1 | ~6,180 |
| M15 | ~24,700 |

If an early period comes in at a fraction of the later rate, **the early data is a different
timeframe wearing your chart's label.**

---

## The trap in checking it — and it caught my own tool

My first version of this check used the **median gap between bars**.

**A median is a majority statistic.** A series that is mostly correct and partly wrong has a
perfect median. Reproduced deliberately:

```
15,450 correct H4 gaps  +  4,680 DAILY gaps,  labelled H4 throughout

median gap        : exactly nominal   ->  the check PASSES
share at nominal  : 76.8%             ->  23% of history is the wrong bar size
commonest wrong gap: 86,400s at 23.2% ->  daily bars, named
density first half vs second : 4.55x
```

**The check reported OK while nearly a quarter of the history was daily bars.** And the
share-at-nominal figure was being computed and printed the entire time — it was simply never acted
on.

So three signals now, all enforced:

| signal | threshold | what only it can see |
|---|---|---|
| median gap | must equal nominal | a wholly mislabelled series |
| **share at nominal** | fails on a significant minority | **a minority of wrong bars** |
| **density, first half vs second** | fails on a material imbalance | **a regime change in the data — and it needs no assumption about trading calendars or holidays, because it compares the data only against itself** |

The output also names the defect now (`86400s at 23.2%`) rather than only failing.

**Verified both ways:** it catches the synthetic case on both new signals, and still passes clean on
a real H1 report — 96% at nominal, commonest non-nominal gap being the daily session break at 3%,
density 1.00×. No false alarm.

---

## The general point

**A median will not show you a minority defect.** If you check anything by its central tendency, you
are blind to the tail — and the tail is where mislabelled, backfilled and synthetic data lives.
