# GMC EA VERIFIER

An independent verification tool for MetaTrader 5 Expert Advisors.

## What It Does

- **Reads source code** — not just backtest reports
- **Detects hidden martingale, overfitting, slippage fragility, and risk gaps**
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

## The number that matters most

**12 EAs built. 10 failed their own pre-registered bar. Around 164 trials, 0 passes.**

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

## Author

Gary Goodison. Findings and methodology are my own work and my own measurements, on my own
accounts, unless a document says otherwise.

No third party's EA is named anywhere in this repository.
