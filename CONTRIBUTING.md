# QUESTIONS, CORRECTIONS AND WHAT NOT TO POST

Questions are welcome. Corrections are more than welcome — see the bottom of this page for why.

---

## ⚠ FIRST: THREE THINGS NEVER TO POST HERE

**This repository is public and its history is permanent.** Deleting a comment does not reliably
remove it — it can persist in caches, email notifications and archives.

| do not post | why |
|---|---|
| **Your account number, login, broker statement or balances** | Permanently public, tied to your name, and useful to exactly the wrong people |
| **Your EA's source code** | Publishing it here destroys whatever value it has. **And I will not read unsolicited source posted in public** — see below |
| **Any password, token, API key or investor password** | Treat anything posted here as published forever |

**If you have already posted one of these, open an issue saying so and I will delete the comment.**
Then assume it was seen: change the password, and if it was an account number, speak to your broker.

---

## WHERE TO PUT THINGS

| what | where |
|---|---|
| **A question** about a check, a finding, or the method | **Discussions** |
| **"I think one of your numbers is wrong"** | **Issues** — this is the most valuable thing you can send |
| **"Your documentation is unclear / contradicts itself"** | **Issues** |
| **A verification enquiry about your own EA** | **Not here.** Nothing about your EA should be public. |

---

## ASKING A GOOD QUESTION

The findings deliberately include the arithmetic so you can reproduce them. So the most useful
question is usually:

> *"I followed finding 0X on my own report and got Y instead of Z. Here is what I did."*

That I can answer precisely. What I cannot answer usefully:

- *"Is my EA any good?"* — not without the report, and the report should not be public.
- *"What settings should I use?"* — I do not know your broker, your account or your risk tolerance.
- *"Will this strategy work?"* — **no backtest can answer that**, which is most of the point of this
  repository. See the limits section in the [README](README.md).

**Describe your setup when it matters:** symbol, timeframe, the date range, your platform build, and
whether the figures came from the Strategy Tester or a live account. Several of the findings here
behave differently depending on window length, and one of them depends on the platform build.

---

## IF YOU THINK A FINDING IS WRONG

**Please say so, and please be blunabout it.**

Two of the four findings in this repository are **my own errors** — things that were live in my own
code for weeks and passed every check I owned. One of them was a claim I had written into a draft
article and was about to publish, which turned out to be false.

So a correction is not an attack on this project. It is the project working.

**What makes a correction easy to act on:**

- the specific number or sentence you think is wrong
- what you think it should be
- how you got there — the arithmetic, the source, or the measurement

**If you are right, the fix goes in and the correction is credited in
[UPDATES.md](UPDATES.md).** The record of what was got wrong stays in the repository; it is not
quietly edited out.

---

## ON OTHER PEOPLE'S EAs

**No third party's EA or author is named anywhere in this repository, and none will be.**

If you ask me in a public thread whether some named commercial EA is a scam, I will not answer
that — not out of evasiveness, but because:

- a figure that does not reproduce is **usually a mistake, not a fraud**, and I cannot tell which
  from the outside
- an author deserves to be asked before adverse findings about their work are published
- and I have made four errors of exactly that kind myself, documented here

**What I will discuss is mechanisms**, not products: what unbounded exposure looks like in source,
why a backtest that calls an external service cannot reproduce, why a grid's equity curve looks
flawless until the one event that ends it. That is more useful to you anyway, because it transfers
to the next EA you look at.

---

## RESPONSE TIME

One person, not a company. **Expect days rather than hours**, and longer at weekends.

Issues that say *"this number is wrong and here is why"* get answered first.
