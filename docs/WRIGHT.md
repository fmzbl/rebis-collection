# wright — writing the program the task actually needs

`modules/wright/program.rebis`. Imports `std/meta`, `std/seam`, `std/imaginary`.

`><` fires a prompt and parses the answer as Rebis, and the parsed program
runs. `std/meta` supplies the structure and phrases nothing; this family is the
wordings that make a model write Rebis rather than prose about Rebis.

## API

| macro | arguments | cost |
|---|---|---|
| `wright-source` | description | one call — returns source as TEXT |
| `wright-run` | description | one write, plus what was written |
| `wright-vetted` | description, shipped, discarded | write, run, one branch |
| `wright-specialised` | task | the task once, one write, plus what was written |
| `wright-until-accepted` | description | per attempt: one write, one run |
| `wright-approved` | made | free — the host's verdict |

## The difficulty is prose, not Rebis

A model asked for a program returns an explanation with a program in it,
fenced, prefaced by "Sure!". Rebis has an unusual syntax and almost no training
data, so every prompt here spends most of its words on the same three
instructions: only source, no fences, no commentary.

The runtime does not paper over the rest. Text that is not a program is a
`GeneratedSyntax` diagnostic **carrying what was written**, so a generator
producing prose is visible rather than silently doing nothing.

Whether a model can actually write good Rebis is an open question this family
does not answer, and the early evidence is not encouraging. Asked to emit one
literal Rebis prompt, llama3.2:3b succeeds stably — and fails just as stably
when a single word inside that prompt is changed, answering the embedded
question instead of writing the program. qwen3:4b fails the working case too.
At this model size the behaviour is word-level capricious.

The quiet failure is the one to watch: a bare word is valid Rebis, so a model
that answers the task instead of writing the program produces something that
parses, runs, answers nothing, and raises no diagnostic. `GeneratedSyntax`
catches prose. It cannot catch that.

## Two acts, and which one you want

`wright-source` returns Rebis **as text**. Nothing runs. Its answer flows
through arrows and out through ports like any other, so a program can be
reviewed, saved, or handed to a host before anything is spent on it.

Everything else here **writes and runs in one act**, because that is what `><`
does — there is no form that hands back generated source without executing it.

So the choice is made by which macro you reach for, and it is the same choice
`git/pr-review` made deliberately when `pr-program` returned source for the
host to run: writing a program and running a program are different acts, and
keeping them apart is what makes a plan reviewable before it costs anything.

## Why every gate is the host

`wright-vetted` and `wright-until-accepted` both send the verdict out through
the seam port. A written program cannot be vetted *before* it runs, so what is
judged is what it produced — and a model asked whether the program it just
wrote is good will say yes.

This is `sci-adjudicate`'s rule and `forge-mechanical-verdict`'s rule, in the
one place it matters most: the thing deciding whether generated code was
acceptable should not be the thing that generated it.

## The shape only `><` can express

`wright-specialised` is the reason the operator earns its place. Every other
decomposition in the library has a shape fixed when the library was written —
`std-dnc` divides, `std-tot` descends, `sci-hypotheses` fans out three ways.
This one lets the **run** decide the shape: a task wanting three independent
readings gets a square, a task wanting a chain gets arrows.

The task is bound with `=` before it is written about, so the program that gets
written and the input it runs on are the same thing. Spliced instead, the
description and the work would be two different readings of it — the confound
`science/method` documents at length.

## Honest limits

- **Cost stops being readable.** What a generated program costs cannot be read
  from the program that generated it. The expansion budget is a bound, not a
  prediction.
- **A model's answer stops being only data.** Under `><` it is executed. A
  program routing untrusted material — a loaded file, a host value, a page a
  tool agent fetched — toward one of these macros has given that material a say
  in what runs. Execution is positional: `wright-run` and the loop already sit
  inside spaces, but containment is not the same as safety.
- **Nothing here measures the generated programs.** Parse rate, correctness,
  and whether a written program beats a hand-written one are all unmeasured.
