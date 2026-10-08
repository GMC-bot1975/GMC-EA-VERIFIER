# FINDINGS

Four things found in ordinary MetaTrader 5 backtests that the reports do not tell you.

**All four are from my own accounts and my own EAs. No third party's EA is named anywhere.** Each
one includes the arithmetic, so you can check it on your own reports rather than taking my word.

| | finding | the short version |
|---|---|---|
| **01** | [The Strategy Tester's Sharpe ratio is not what most people think](01-sharpe-ratio.md) | It is computed on **per-bar equity returns with idle bars dropped**, not on your trade results. Mine read 4.57 one way and 2.65 the other — and **which one is higher reverses** depending on the window length |
| **02** | [A broker served me daily bars inside an H4 chart for eighteen years](02-mislabelled-timeframe.md) | ~260 bars a year before 2016, ~1,545 after. **3,819 of them exactly 24 hours apart.** History Quality still reported 99%, because it does not measure that |
| **03** | [My financing cost was understated by 4.553% a year](03-financing-carry.md) | I was charging the **symmetric** part of the swap and omitting **directional carry**. A long position pays 5.335%/yr of notional, not 0.782% |
| **04** | [Winners gave back 61% of their peak profit](04-profit-giveback.md) | Average peak **+140.42**, average realised **+54.28**, across 33 closed positions. A per-trade Sharpe cannot see this at all |

---

## Two of these were my own errors

Findings 01 and 03 are not discoveries about other people's work. **They are defects that were
live in my own code for weeks**, passing every check I owned, until something independent was
computed and the two numbers disagreed.

That is included deliberately. The question worth asking of any verification service is not
*is your tool clever* — it is **what happens when your tool is wrong.** These documents are the
answer.

Finding 01 in particular records a claim I had written into a draft article and was about to
publish, which turned out to be false. It was caught by searching properly instead of asserting.
