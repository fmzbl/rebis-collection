# numerology — choosing the same way twice, for nothing

`modules/numerology/name.rebis`. Imports `std/measure`.

A gematria is a deterministic function from text to a number: letters become
figures, the figures are summed, the sum reduces to one. It is the oldest such
reading there is, and mechanically it is an ordinary hash — which is exactly
what makes it useful, and exactly the limit of what it can claim.

## API

| macro | arguments | cost |
|---|---|---|
| `name-value` | text | nothing |
| `name-shares` | a, b | nothing |
| `name-splits` | text, left, right | one branch |
| `name-breaks-tie` | a, b, text | one branch |
| `name-numbered` | text, work | what `work` costs |

## What it is for

**A free, reproducible choice.**

Rebis already has one free deterministic judge: the symbol mediator, which
scores by holonomy. This is a second, on a different measure, in the same slot.

Where a run must pick and nothing about the pick needs to be *right* — a tie to
break, an exploration to shard, a selection that must survive a re-run byte for
byte — a model call buys nothing and a reading costs nothing.

`name-breaks-tie` is the honest case. When two options have been judged and
neither won, something must still choose. Asking a model again gives a different
answer than it gave the first time, which makes the run unrepeatable for no
gain. This chooses badly but **identically**, which is strictly better than
choosing badly and differently.

## What it is not

Nothing here claims the number means anything.

The collection says this about Jung already and it is truer here: these are
composition tools, not diagnostic tests. A route chosen by a name is
*reproducible*, not *correct*, and every macro is written so nothing else could
be inferred from it.

## The gate

Not accuracy — **determinism**. Same text, same figure, same branch, forever.

That is testable, and it is the whole of what is promised. The moment a macro
here starts claiming its choice is better than another, it has become
unfalsifiable and should be cut.

## Why it lives in the language rather than the host

Because the reduction is arithmetic, and the numeric plane made arithmetic free.

`name-value` is `(std-root (std-weight text))` — a crossing that reads text as
quantity, and a modulus that reduces it. Both cost nothing, both are exact, and
neither fires a model. Before the plane existed this would have been a host
capability or a model call; now it is two macros over operators that were
already there.
