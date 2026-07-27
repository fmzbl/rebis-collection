# forge — the coding repair loop

`modules/forge/repair.rebis`. Imports `std/spiral`, `std/seam`, `std/shape`.

`std/spiral` is a structural module: it carries the Fibonacci restart topology
and refuses to phrase anything, asking its caller for a frame, a distiller, a
worker, and a gate. `forge` is the other half — the phrasings that turn that
ladder into a repair loop, plus the wiring that hands them over. If you want to
see how the two layers are meant to relate, read `forge-repair` first.

## API

| macro | arguments | cost |
|---|---|---|
| `forge-reproduce` | task | one call |
| `forge-hypotheses` | judge, task | three readings, one mediation |
| `forge-root-cause` | judge, task | reproduce, then three readings and a mediation |
| `forge-attempt` | budget | none — it is a directive frame, not a working |
| `forge-verdict` | attempt | one call per banished attempt |
| `forge-mechanical-verdict` | work, shipped, banished | work once; the decision is free |
| `forge-repair` | work, gate, cause, seed, grown | per rung: attempt spliced twice, one verdict on rejection |
| `forge-postmortem` | judge, outcome | three readings, one mediation, one evidence answer |

## The loop

```rebis
(
  (# forge/repair)
  (~ repair (directive) (-> directive "Apply the smallest fix and verify it."))
  (~ accepted (attempt) (std-binary (-> attempt "Do the tests pass now?")))
  (forge-repair repair accepted
    (forge-root-cause cheapest-check "Cancelling a run leaves the child alive.")
    "........" "............."))
```

The cause is established once and flows into the first attempt. Every rejected
attempt is **banished** — its context is discarded whole and only
`forge-verdict`'s five lines cross the gap — and the next rung is drawn by the
twin, in the opposite orientation. Budgets are lengths of text rather than
numerals, so the ladder is assembled by `$` during expansion and costs nothing;
a host enforcing a step budget reads the rung's length.

## Prefer the mechanical gate

A model asked whether its own patch is good will usually say yes. `gate` can be
any one-argument 0/1 judge, but the one worth using is the host's:

```rebis
(forge-mechanical-verdict (repair task) (forge-commit task) (forge-log task))
```

The working runs, then the host answers through `std/seam`'s port with an exact
0 or 1 — it watched whether a file changed and whether the tests passed. The
decision costs no model call and no answer can argue with it. Everything else
in this family prepares a question; this is the part that settles it.

## Cost and bounds

The ladder has no base case: the gate stops it, and the runtime's expansion and
model-call budgets are the hard backstop. An accepted first rung costs one
ladder step, so the schedule is free when the first attempt is right. Set
expansion and model-call limits before running it against a gate that might
never accept.
