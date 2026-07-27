# science — claims that have to survive something

`modules/science/method.rebis`. Imports `std/sieve`, `std/seam`, `std/shape`.

A model will produce a confident account of almost anything, and confidence is
the one signal that carries no information. What carries information is a claim
that **forbids an observation**. Every protocol here is built around that
asymmetry: it ends by asking what would have to be true for the claim to be
wrong, and what it would cost to look.

This family does not pretend a language model can run an experiment. It cannot.
What it can do is state a prediction precisely enough that a host, a test, or a
person can check it — and `sci-adjudicate` is where that check comes back in.

## API

| macro | arguments | cost |
|---|---|---|
| `sci-hypotheses` | judge, observation | three readings, one mediation |
| `sci-prediction` | hypothesis | one call |
| `sci-experiment` | judge, hypothesis | three designs, one mediation |
| `sci-falsify` | claim | one call |
| `sci-tested` | forbids, survives, claim, body | one or two judges, lazily |
| `sci-adjudicate` | claim, upheld, overturned | design once; the verdict is free |
| `sci-replicate` | judge, claim | two derivations, one mediation |
| `sci-record` | finding | one evidence-shaped call |

## The shape of a settled question

```rebis
(
  (# science/method)
  (~ forbids (c) (std-binary (-> c "Does this claim forbid any observation?")))
  (~ survives (c) (std-binary (-> c "Does it survive its strongest refutation?")))
  (sci-tested forbids survives claim
    (sci-adjudicate (sci-experiment cheapest-decisive claim)
      (sci-record claim)
      (sci-hypotheses most-discriminating claim))))
```

`sci-tested` holds the claim to both bars before any budget is spent on testing
it. The sieve is lazy, so a claim that forbids nothing costs exactly one judge
call and the steelman is never paid for — which is the point of ordering the
cheap, decisive judge first.

`sci-adjudicate` then hands the experiment to the host. An overturned claim does
not merely fail: it flows back into `sci-hypotheses`, so a refutation restarts
the search rather than ending it.

## Why the verdict leaves the program

Every other gate in the standard library asks a model to judge an answer, and a
model judging an answer can be talked out of its verdict by that answer. A port
cannot. `sci-adjudicate` is the only macro in this family that can settle
anything; the rest exist to make the question precise enough to be worth asking
of something that actually knows.

## Honest limits

- A deterministic judge is a topical gate, not a correctness oracle.
- `sci-replicate` reduces shared-assumption agreement; it does not eliminate it.
  Two derivations from the same model share a prior no instruction removes.
- `sci-record` fixes what was measured and what would overturn it. It does not
  make the measurement true.
