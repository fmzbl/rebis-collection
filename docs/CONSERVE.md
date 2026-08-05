# conserve — things a chain must not change

`modules/conserve/law.rebis`. Imports `std/amend`.

`@` holds a rule across every arrow in a scope. `std/amend` supplies the
structure and phrases nothing; this family is the wordings for the one class of
rule worth spending an operator on.

## API

| macro | arguments | cost |
|---|---|---|
| `law-preserves` | subject | one call per arrow |
| `law-adds-nothing` | — | one call per arrow |
| `law-drops-nothing` | — | one call per arrow |
| `law-answers-the-same-question` | — | one call per arrow |
| `law-conserved` | subject, work | the work, plus one call per arrow |
| `law-faithful` | work | the work, plus **two** calls per arrow |

## The class of rule that works

A **conservation**, not a judgement. Not *"is this step good"* — that belongs in
a mediator, and asked at every arrow it returns 1 forever while costing a call
each time. A conservation asks whether the thing that must not change survived
the step: a quantity, an identity, a constraint that held before.

Those are the rules a model can check reliably, because they are about the two
texts in front of it rather than about the world. Every macro here is phrased to
ask about a **difference**, and to answer with one character.

## The failures they catch

Each of the three unnamed laws catches something a chain does that no single
step looks guilty of:

- **`law-adds-nothing`** — a stage quietly invents a number, a name, a cause.
  Invisible at the end, because the invented fact reads like the others.
- **`law-drops-nothing`** — a stage loses a constraint at step two and produces
  something coherent and wrong at step five.
- **`law-answers-the-same-question`** — drift. Every stage is a reasonable
  response to the one before it, and stage six answers a question nobody asked.

Held at every arrow, each is caught **at the step that introduced it** rather
than at the end, which is the difference between a diagnosis and a symptom.

## Prefer the mechanical version

A model comparing two texts is not a verifier. Where a mechanical check exists —
a parser, a balance, a test suite, a diff — `std/amend`'s
`std-holding-mechanically` routes the same question to the host through the seam
port: it costs nothing, and it cannot be talked out of its verdict by the step
it is judging.

These macros are for the case where no such check exists. That is a real case,
and it is also the weaker one.

## It is not free, and `+` is

Both scope over a subtree, so the two look like the same idea in different
currency. They are not. `+` **rewords** every prompt inside and fires nothing —
it costs only the tokens it adds. `@` **inserts a call** at every arrow and can
stop the run.

Reach for `+` freely. Reach for `@` when the rule is worth a call per edge.

## Cost, said plainly

One call **per arrow**, not per program. A four-stage chain is three arrows and
three checks — doubling the cost of the chain. `law-faithful` nests two
invariants and therefore costs **two** calls per arrow.

That is worth it when the chain is the deliverable. It is not worth it inside a
loop, and a `std-until` running a checked chain fifty times has bought fifty
times the checking.

A refusal stops the scope, so a chain that breaks early costs less than a whole
one — the one place where failing is cheaper than succeeding.
