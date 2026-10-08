# THE METHOD

The whole of it is one sentence: **nothing is believed on a hunch, and nothing is re-tuned after it
fails.**

---

## 1. PRE-REGISTRATION

Before a candidate is tested, the test is written down:

- what mechanism is being tested, in enough detail that it cannot be reinterpreted later
- **what number it has to beat, exactly**
- how many configurations will be tried in total
- what would make the result void

Then the document is **hashed**, and the hash is stored separately. Everything above the results
line is frozen. The results are appended below it, afterwards, and nothing above may change.

**An integrity check re-computes every hash on a schedule.** If a single character above a results
line has moved, it fails loudly. At the time of writing it reads:

```
[OK] locks  56 pre-registrations: decision criteria unchanged since locking
```

### Why bother

Because the failure it prevents is invisible and nearly universal. The sequence is:

1. test a strategy
2. it misses the bar
3. change a parameter
4. it clears the bar
5. report step 4

Nobody doing that thinks of themselves as dishonest. Each individual step is reasonable. But the
number reported at the end is **the best of many attempts presented as the result of one**, and it
will not survive contact with live money.

Pre-registration does not make you a better researcher. It makes that specific sequence
**impossible to perform without leaving a trace.**

---

## 2. DECLARE THE TRIAL COUNT, AND MEAN IT

If you try 47 configurations against a 5% significance bar, **roughly two will clear it by chance
alone.** They will look exactly like the real thing.

So the trial count is declared in advance and the significance is corrected for it. Undeclared
trials are the single commonest lie in a backtest — usually an honest one, because the discarded
attempts genuinely do not feel like part of the experiment.

They are.

---

## 3. THE PROVABILITY CEILING

```
t = Sharpe × √years
```

This is arithmetic, not opinion, and it is brutal:

| what you have | what it can prove |
|---|---|
| 25 years of history | a Sharpe of about **0.60**, no better |
| a documented single-market trend edge (Sharpe 0.24–0.40) | needs **56 to 156 years** to prove |

**So running more trials against a higher bar is not a strategy.** If the data cannot carry the
claim, no amount of testing will make it. The honest move is to say the question is unanswerable
with the sample available — and most questions in retail algorithmic trading are.

One exception: **a cost or broker-rule edge is deterministic, not statistical.** A swap
mis-specification is provable the day it is measured, because it is a fact about a contract rather
than an estimate from a sample.

---

## 4. RULES THAT ARE NOT NEGOTIABLE

- **Do not re-run a failed test with different parameters and report the better number.**
- **Do not soften a pass criterion after seeing the result.** If the bar was wrong, the honest
  route is a new registration with the new bar, declared as a new trial — not an edit to the old
  one.
- **Do not deploy anything that has not cleared its registered bar.**
- **Record the failures.** A research record is only worth something if the deaths are in it. Ten
  failures and one pass is a finding. Ten failures quietly deleted and one pass is a brochure.

---

## 5. SPEC-VERIFIED IS NOT DEAL-VERIFIED

Say which one a number is.

Reading a broker's specification tells you what *should* happen. A real position, held through the
event, tells you what *does*. These disagree more often than you would expect.

A worked example from this project: the specification implied a particular night carried triple
financing. Reading the spec got the **day of the week wrong**, and it stayed wrong for two days,
because of a weekday-numbering difference between two systems. It was settled by holding a small
real position through the rollover and reading the actual charge — predicted +0.605, actual +0.61.

**One real trade closed in an evening what two days of specification-reading had got wrong.**

---

## 6. FOUR LAYERS FOR CATCHING YOUR OWN ERRORS

A self-contained system cannot verify its own premises. Every internal check compares something to
an expectation that is *also inside the system*, so **a wrong belief held consistently passes every
test you own.**

Four ways out, strongest first — which is the opposite of the order most people build them in:

| | layer | what it does |
|---|---|---|
| **1** | **Eliminate** | Derive it, don't store it. An assumption you do not hold cannot be wrong. Read the swap rate from the broker; never hardcode it |
| **2** | **Redundancy** | Compute the same quantity two independent ways and fail on divergence. **The only layer that catches errors nobody anticipated** — you don't need the right answer, just two paths that would fail differently |
| **3** | **Reconcile** | Compare what the model predicts to what the broker actually did. Reality is the one reference you cannot be wrong about |
| **4** | **Register** | For what is left — definitions and semantics — record the source and the date, and fail on anything load-bearing that has no source |

**Documentation is the weakest of the four and it is where everyone starts.**

Two of the four findings in this repository were caught by layer 2, and both were errors in my own
code that every other check had been passing for weeks.

---

## 7. THE HONEST LIMIT

**None of this catches a premise nobody thought to question.**

One finding in this repository sat undetected across eighteen years of test data because the
*label* on the data was never doubted. Layer 2 is the best defence precisely because it does not
require suspicion — but it only guards the quantities you chose to compute twice.

The residual risk is real and is not claimed away. The practical mitigation is to make re-deriving
things so cheap that it happens casually: every constant replaced by a function call, every
assumption with a one-line command that re-checks it.

**Cheap to check is the only thing that makes checking happen.**
