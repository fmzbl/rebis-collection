# source — a program that is about itself

`modules/source/self.rebis`. Imports `std/source`, `std/seam`, `std/shape`,
`std/imaginary`.

`<>` is the program, as syntax. `std/source` supplies the structure and phrases
nothing; this family is the other half — the wordings that make those shapes a
self-examination.

The discipline is the one that keeps self-reference from being flattery. A
model asked to review its own program will call it well-structured. So every
protocol here either asks for something a compliment cannot satisfy — a
specific line, a named failure, a rewritten form, a number — or hands the
verdict to something that is not the model.

## API

| macro | arguments | cost |
|---|---|---|
| `self-explain` | — | one call |
| `self-weakness` | — | one call |
| `self-cost` | — | one call |
| `self-revise` | instruction | one call |
| `self-improved` | judge | the weakness once, one mediation |
| `self-until-accepted` | work | per pass: the work once; the verdict free |
| `self-attempted` | subject | one write, plus running what was written |
| `self-climbed` | subject, work | two writes, two runs, then the work |
| `self-recorded` | — | one call, kept past the run |

## Sight, not authority

Nothing here applies a revision. `self-revise` returns source **as text**, and
returning it is all it does — no file is written and no running program is
replaced.

That is not caution, it is the shape of the thing. Kaos's God Agent may revise
a running program because it works in an isolated bridge and acts only through
parsed, validated actions compared against the editor revision before they
merge. A form inside the run has neither: it is not isolated from what it
observes, and its output is prose, so nothing can check what it asks for. A
supervisor inside the run is a participant with a large prompt.

So the loop is: the program proposes, a person or a host disposes.

## Why the critique is imaginary

`self-improved` runs its critique inside `{ }`, and this is the clearest case
in the whole collection for that boundary.

A criticism of a program is detailed, specific, and phrased in the program's
own vocabulary. Left in the record it outranks the revision at every later
recall — so a run that improved itself would spend the rest of its life
remembering what was wrong with the version it no longer is. The critique
happens in full and is forgotten; the proposal crosses out.

## The loop that needs a port

`self-until-accepted` is `std/source`'s `std-until` with the only gate worth
using on it. A model asked whether its own program is finished says yes, and a
loop over the *whole program* is exactly the loop where that answer must not be
trusted. The verdict leaves through the seam port, costs no call, and cannot be
argued with by the draft it is judging.

A host that never answers 1 has written an unbounded program. The expansion
budget is the backstop, not the design — self-application has no base case of
its own.

## The ratchet

`self-attempted` is where `<>`, `><` and `{ }` meet, and the shape is worth
reading closely because it is not the obvious one.

The obvious design for self-improvement is a chain: v1 writes v2, v2 writes v3,
each generation reading itself. Rebis deliberately cannot do that — `<>` is the
RUN's program, so a generated program that reads `<>` reads the original. A
first reaction is that this is a limitation.

It is not. Look at what accumulates instead:

```rebis
(self-climbed "summarising incident reports" (do-the-real-work))
```

Each attempt writes a new version of the program and runs it, **inside an
imaginary space**, so nothing it produced becomes evidence. What it learned is
marked to the plane, which the next attempt reads. So:

- every attempt starts from the same **reviewed** program, not from an
  unreviewed descendant of the last mutation;
- every attempt knows what the previous ones **ruled out**;
- the record ends up holding the real work and nothing else — no draft
  program, no failed run, no critique.

That is hill-climbing with memory. The chain design gives you drift: by v5 you
are running something no one read, derived from something no one read. Here the
thing that accumulates is the LESSON, and the program stays the program.

Which is the same boundary the rest of this family keeps. A program proposes;
it does not become. If you want a successor to actually replace the original,
that crosses a run — the host keeps it, and the next run's `<>` is the new one,
having been looked at first.

## Honest limits

- **A program can read itself; it cannot read its run.** The trace is
  deliberately unreachable, because it holds every firing inside every
  imaginary space: a form that read it would turn speculation into evidence.
- **`self-cost` is checkable, which is the point** — Rebis makes cost readable
  off the page, so a wrong count is a wrong answer rather than an opinion. It
  is still a model counting.
- **`self-climbed` does two attempts, not a loop.** A self-rewriting loop whose
  stop condition is a model does not terminate on any principle. For a real
  loop use `std/source`'s `std-until` with a mechanical gate.
- **Nothing measures whether the rewrites are any good.** The shape is sound;
  whether a model writes better Rebis than the program it was given is an
  open empirical question, and this family does not answer it.
- **`self-explain` describes the program, not the run.** What actually happened
  is `?`, and what a supervisor saw is the God Agent's.
