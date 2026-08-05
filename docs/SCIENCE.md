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
| `sci-experiment` | judge, hypothesis | the hypothesis once, three designs, one mediation |
| `sci-falsify` | claim | one call |
| `sci-tested` | forbids, survives, claim, body | one or two judges, lazily |
| `sci-adjudicate` | claim, upheld, overturned | design once; the verdict is free |
| `sci-replicate` | judge, claim | the claim once, two derivations, one mediation |
| `sci-rerun` | claim, design | the claim once, the design twice, one mediation |
| `sci-record` | finding | one evidence-shaped call |
| `sci-established` | finding | one call; kept past the run |

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

## The claim has to hold still

Every comparison in this family — three experiment designs, two derivation
routes, a rerun — is only evidence if the thing being compared is *the same
thing* in each arm. That is harder than it sounds in a language where a
parameter is syntax: writing the claim into three branches derived it three
times, and a model asked the same question three times gives three answers. The
mediator was then comparing experiments for three different theories and
reporting agreement or disagreement about none of them.

Every such macro here binds its subject with `=` before it branches:

```rebis
(= result the-claim
   ([judge] (route-a result) (route-b result)))
```

One firing, and both arms provably about one claim. `sci-replicate` holds the
claim and varies the route; `sci-rerun` holds both and varies only what a model
does twice, which is the noise floor the others are read against.

## Findings that outlive the session

`sci-record` writes a result up so it can be checked. `sci-established` also
keeps it: the write-up goes into the project's durable memory, so a later run's
flashback on the subject recalls the finding instead of re-deriving it.

```rebis
(sci-established (sci-adjudicate the-claim upheld overturned))
```

Keep what was established, not what was drafted — recall is topical, so a
memory full of drafts is a memory nothing can be found in.

---

# science/evidence — when the observations are numbers

`science/method` is about claims that forbid an observation. `science/evidence`
is about what you do once the observations are quantities: whether a
measurement clears a bar, whether replications agree closely enough to mean
anything, and how large the effect actually was as opposed to how confidently
it was described.

```rebis
(# science/evidence)
```

The two halves fail in opposite directions. A model asked *is this
significant?* answers yes, fluently, about noise — the asymmetry `sci-falsify`
is built around. A model asked to *compare two numbers* fails more plainly: it
is doing arithmetic, badly, at the price of a call. Neither settles a quantity.

So every judgement here is arithmetic in the `| |` plane and costs **nothing**.
The run counts, and a count cannot be argued with by the work it is judging.

## Getting a number costs; using it does not

`evi-quantify` is the only macro here that fires, and it fires twice — the
reading, then the shaping call that turns a sentence into a bare figure.

```rebis
(evi-quantify (the-report) "How many distinct failures are described?")
```

That is the toll. Everything downstream is free:

```rebis
(evi-clears measured 3 (write-it-up) (keep-looking))
(evi-as-predicted latency 200 20 (report-it) (revise-the-claim))
```

`evi-as-predicted` asks the question a single threshold cannot: not above or
below, but **near**. State the tolerance before measuring — one chosen
afterwards is not a test.

## Effect size, which write-ups bury

```rebis
(evi-effect 200 170)                    ; 30
(evi-relative-effect 200 170)           ; 3/20, exactly
(evi-worth-reporting 200 170 (std-portion 1 10) (say-it) (drop-it))
```

Choose the floor from what the change is *for*. A model has no way to know
which differences matter, and will describe a two percent shift as substantial
if asked to describe it.

## Do the replications agree

```rebis
(evi-consistent |([min] a b c)| |([max] a b c)| 2 (report-it) (chase-it))
(evi-central |([sum] a b c)| |([count] a b c)|)
```

Nothing here is arity-locked. The fold is written at the mediator, which takes
as many branches as you give it, so adding a fourth replication changes only
those two lines. Three runs agreeing within a point is a finding; three
scattering across the range is also a finding — about the measurement — and
averaging them would hide it.

## The honest limit

**There is no significance test, and there will not be one until the plane can
square.** Variance needs each deviation multiplied by itself, and `+` frames:
it shifts every quantity in its scope by one amount, which is a translation and
not an arbitrary function.

What this family offers instead is the spread and the mean absolute deviation —
in the units of the observations rather than their squares, and therefore
checkable against the raw numbers in the trace. A p-value nobody can recompute
from the trace would be worse than no p-value.

## The whole protocol

```rebis
(evi-settled (the-subject) centre scatter effect 2 (std-portion 1 10))
```

Both gates run before anything is written, and each failure says which one it
was. The order is the point: a result whose replications scattered is not made
publishable by a large effect, and a tight result whose effect is negligible is
not made interesting by being tight.
