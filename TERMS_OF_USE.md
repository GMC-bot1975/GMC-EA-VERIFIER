# Terms of Use — GMC EA VERIFIER

**Last updated: 8 October 2026**

## 1. Purpose

This public repository describes the methodology, findings, and capabilities of the GMC EA
VERIFIER — an independent verification tool for MetaTrader 5 Expert Advisors.

## 2. What Is Shared

- ✅ General methodology and verification principles
- ✅ What each of the eleven checks tests, and why it exists
- ✅ Public findings, example reports, and educational content
- ✅ Project updates and roadmap
- ✅ **The arithmetic behind every published figure, so it can be checked rather than believed**

## 3. What Is NOT Shared

- ❌ **Source code** — proprietary, unpublished, and held as a trade secret
- ❌ **Implementation details** — thresholds as coded, calibration data, parser internals, the
  automated integrity and reconciliation layer
- ❌ **Core system architecture** — private and protected
- ❌ **Any client's EA, report, results, or identity** — see
  [PRIVACY_AND_DISCLOSURE.md](PRIVACY_AND_DISCLOSURE.md)

## 4. Intellectual Property

Copyright in this repository's documents belongs to **Gary Goodison, trading as GMC EA VERIFIER**.

Two different things are protected in two different ways, and the distinction matters:

| | what protects it |
|---|---|
| **The documents here** — the wording, structure, tables and worked examples | **Copyright.** Licensed to you under the terms in section 5 |
| **The tool itself** — the source code, and the implementation detail listed in section 3 | **Copyright and trade secret.** Nothing here grants any right to it, and nothing here discloses it |

**On methods and formulae, stated plainly because overclaiming would be worse than claiming
nothing:** copyright protects the *expression* of an idea, not the idea itself. Standard statistics
— `t = Sharpe × √years`, a Bonferroni correction, a log-return Sharpe — belong to nobody and are
not claimed here. **What is proprietary is the working system**: how the checks are implemented,
calibrated, sequenced and defended against the failure modes documented in this repository.

## 5. Permitted Use

The documents in this repository are licensed under
**[Creative Commons Attribution-NonCommercial-ShareAlike 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)**.
In plain terms:

**You MAY:**
- ✅ Read, quote, share and link to this repository
- ✅ Reference the methodology with clear attribution to Gary Goodison and a link back here
- ✅ **Apply the published checks to your own reports, for your own purposes** — this is actively
  encouraged, and the findings are written so you can
- ✅ Translate or summarise it, releasing your version under the same terms

**You MAY NOT:**
- ❌ Sell this content, or include it in a paid product or service
- ❌ Build or operate a **commercial or competing verification service** on this material
- ❌ Reverse-engineer, reconstruct or reimplement the GMC EA VERIFIER system for commercial use
- ❌ Present this work, or conclusions drawn from it, as your own

**The boundary in one sentence: checking your own EA with these methods is the point; charging
other people to do it is not.**

For any commercial use, ask. Written permission is required and is sometimes given.

## 6. No Financial Advice, and No Guarantee

All findings, examples and test results here are for **educational and demonstration purposes
only**. They are **not** financial advice, not a recommendation to trade, and not an inducement to
buy or use any product.

- **Trading carries risk of loss.** Past performance does not indicate future results.
- **A clean verdict from this tool means "not yet disqualified" — nothing more.** No backtest can
  prove an EA works. This is stated throughout the documentation and is not a disclaimer bolted on
  afterwards.
- **Every figure published here is a measurement taken on my own accounts, with my own EAs, against
  my own broker.** Your broker, symbol, account currency and platform build will differ. Check your
  own.
- **The documentation is provided "as is", without warranty of any kind.**

## 7. Corrections

**If something here is wrong, please say so.** Two of the four published findings are my own
errors, documented as such.

Corrections are accepted through
[Issues](https://github.com/GMC-bot1975/GMC-EA-VERIFIER/issues), credited in
[UPDATES.md](UPDATES.md), and **the record of what was got wrong stays in the repository** rather
than being quietly edited out. See [CONTRIBUTING.md](CONTRIBUTING.md) — including the three things
never to post in public.

## 8. Changes to These Terms

These terms may change. The version history is public in this repository's commit log, so any
change is visible and dated.

---

**Copyright © 2026 Gary Goodison, trading as GMC EA VERIFIER. All rights reserved.**

Documents licensed under CC BY-NC-SA 4.0. The verifier's source code is not published and is not
licensed under these terms.
