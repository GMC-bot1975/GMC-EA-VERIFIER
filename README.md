# GMC EA VERIFIER

An independent verification tool for MetaTrader 5 Expert Advisors.

## What It Does

- **Reads your backtest report, trade history or live account** — and tells you what it actually proves
- **Detects hidden martingale, overfitting, slippage fragility, and risk gaps**
- **Structural review of your source code**, where you choose to provide it
- **Hash-locked pre-registration** — results cannot be tampered with
- **Plain English reports** — no jargon

## Status

Forward testing live.

---

## What this repository is

This repository holds the **method and the findings**. The verifier's source code is not published.

| here | not here |
|---|---|
| how the checks work, and why each one exists | the tool's source code |
| the standard a result has to clear before it is believed | any client's EA, report or results |
| findings that are reproducible by anyone | account numbers, balances, broker data |

### Why the code is not public

The checks are described here in enough detail that anyone can reproduce them by hand — that is
deliberate, and several of the findings below include the arithmetic so you can. The tool itself is
the service.

---

## Start here

| | |
|---|---|
| **[docs/methodology.md](docs/methodology.md)** | The standard. Pre-registration, hash-locking, and why a result that was not declared in advance is worth less |
| **[docs/what-it-checks.md](docs/what-it-checks.md)** | All eleven checks in plain English, and the real failure each one was written after |
| **[docs/findings/](docs/findings/)** | Four things found in ordinary MT5 backtests that the reports do not tell you |
| **[examples/sample-report.md](examples/sample-report.md)** | A full verdict on one of my own EAs. It failed |

---

## What this does for you

**Your EA looks great on paper. That's the problem.**

A backtest is the best of everything you tried, measured on the data you tuned it against. It is
built to look good. The question is what is left once that is stripped out — and that is the only
question this answers.

**Truth over backtests.**

I will not tell you an EA is robust, because no backtest can show that. What I will tell you is what
your evidence is actually worth: whether the data is real, whether the sample is big enough to mean
anything, whether it survives the half it was never fitted to, whether it beats simply holding the
instrument, and whether the edge is bigger than the cost of trading it.

**Hidden curve fitting is the thing that costs people money**, and it rarely looks like a mistake. A
strategy fitted to its own history produces a confident, smooth, entirely convincing equity curve.
The checks here are built to separate that from an edge — out-of-sample consistency, correction for
how many configurations were tried, and whether a handful of trades carry the whole result.

**Don't guess — verify.**

Eleven checks on your backtest report, your trade history or your live account. One page, plain
English, no jargon, and the figures shown so you can check the arithmetic yourself. Where you
choose to send the source code, I will read it for the things a report cannot show: unbounded
exposure, no stop, or a strategy that quietly depends on something it should not.

**Your capital deserves evidence, not promises** — including from me. Which is why the criteria are
hash-locked before each test runs, so no result can be moved afterwards, and why every one of my own
failures is published on this page rather than quietly deleted.

---

## The number that matters most

**45 strategies built. 56 pre-registered tests. Not one passed.**

Every bar was written down and hash-locked *before* the test ran, so none of them could be moved
afterwards.

That is not a sales pitch, and it is not modesty. It is the only honest thing a person in this
field can say, and it is the reason this tool exists: **almost everything fails, and most of it
fails for reasons the backtest report does not mention.**

If a verification tool never fails anything, it is not measuring. If it fails everything, it is not
discriminating either — so the findings here include the cases where my own assumptions turned out
to be the thing that was wrong.

---

## What this tool cannot do

Stated up front, because a checker that oversells itself is the problem it claims to solve:

- **It cannot tell you an EA works.** No backtest can. A clean verdict means *"not yet
  disqualified"* — nothing more. The live forward record is the evidence.
- **It cannot price your broker's execution.** Costs are per-account and per-symbol. Where a
  threshold was calibrated on one instrument, the reports say so.
- **It cannot see what nobody thought to measure.** Two of the four findings below exist because I
  assumed something for weeks before checking it.

---

## Terms, Privacy and Disclosure

| | |
|---|---|
| **[TERMS_OF_USE.md](TERMS_OF_USE.md)** | What is shared and what is not. Documents are **CC BY-NC-SA 4.0** — quote them, cite them, apply the checks to your own reports. **The tool's source code is unpublished, proprietary, and held as a trade secret** |
| **[PRIVACY_AND_DISCLOSURE.md](PRIVACY_AND_DISCLOSURE.md)** | This repository collects nothing. If you send me an EA it is **confidential by default and never named publicly**. Includes the conflict you are entitled to know about: **I trade my own EAs**, and what I commit to because of it |
| **[CONTRIBUTING.md](CONTRIBUTING.md)** | Where to ask questions, and the **three things never to post in public** |

**The boundary in one sentence:** checking your own EA with these published methods is the point;
charging other people to do it is not.

**Not financial advice.** Trading carries risk of loss. A clean verdict means *"not yet
disqualified"* — nothing more.

## Author

Grayson Goodison, trading as GMC EA VERIFIER. Findings and methodology are my own work and my own
measurements, on my own accounts, unless a document says otherwise.

No third party's EA is named anywhere in this repository, and none will be.

**Copyright © 2026 Grayson Goodison. All rights reserved.**
