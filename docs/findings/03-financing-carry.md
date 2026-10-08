# 03 — MY FINANCING COST WAS UNDERSTATED BY 4.553% A YEAR

My own error, live in five scripts and three documents for weeks. It passed every check I owned.

---

## The mistake

I was charging gold financing at **0.782% a year of notional**, a figure I had measured once and
then hardcoded.

That number is real, but it is **only the symmetric part of the swap.** A CFD swap has two
components:

| component | gold, measured on this account |
|---|---|
| **symmetric financing** — paid by both sides | **0.782%/yr** |
| **directional carry** — paid by one side, received by the other | **4.457%/yr** |

So:

```
a LONG  position PAYS      0.782 + 4.457  =  5.335 %/yr
a SHORT position RECEIVES  4.457 - 0.782  =  3.660 %/yr
```

**I was charging a long-only strategy 0.782% when it was actually paying 5.335%** — understated by
**4.553 percentage points a year**, every year of every backtest.

---

## Why this is worse than it looks

On a long-only strategy held through a multi-year test, 4.553%/yr compounds into a very large
number — and it lands directly on the one comparison that matters most, the **buy-and-hold
benchmark**, which is itself a long position paying exactly this cost.

Get this wrong and you can show a strategy beating buy-and-hold when it does not, or losing to it
when it does not. Either way the headline conclusion flips.

---

## How it was caught

**Not by any check.** It was caught by **deriving the number instead of recalling it.**

I was making the cost model work per-symbol rather than gold-only, which meant reading the
broker's own fields instead of using my stored constant:

```
swap_long  -59.340 points/night
price       4,138.98
mode        SWAP_MODE_POINTS

-> per night : 59.340 × point / price
-> annual    : × 365
-> 5.228 %/yr
```

**The derived figure disagreed with the stored one by a factor of about seven, and that is how I
found out.**

---

## How to check yours

Read `swap_long` and `swap_short` live from the symbol rather than assuming. In MQL5 that is
`SymbolInfoDouble(symbol, SYMBOL_SWAP_LONG)` with `SYMBOL_SWAP_MODE` to tell you the units —
points, percent, or account currency are all possible and they are not interchangeable.

**Then check the sign and the direction.** A negative swap is a cost. A long and a short on the same
instrument do **not** pay the same thing, and a long-only backtest that charges the symmetric figure
is understating its costs.

**And verify against the real charges on your statement.** The swap field tells you the rate; the
`swap` value on your closed deals tells you what you were actually charged. If those two disagree,
trust the statement.

---

## The general point, which is the whole reason this is a finding

**If a number can be derived at runtime from an authority, never store it.**

An assumption you do not hold cannot be wrong. Every hardcoded constant in a cost model is a future
defect waiting for the conditions to change — a different symbol, a different broker, a different
account currency, a rate the broker revised last Tuesday.

The verifier now derives financing **per symbol, live, and prints the derivation** in its output, so
a wrong one is visible rather than silent. Two further constants were removed the same way after an
audit found them: a point value measured on gold and applied to every instrument, and a stored
benchmark ratio that was being shown to strategies on unrelated symbols.
