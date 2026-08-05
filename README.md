
Reusable Rebis modules shared by Kaos and other Rebis runtimes.

This is intentionally a source collection, not a Rust framework. The library
is the `.rebis` tree under `modules/`. A file such as
`modules/archetypes/shadow.rebis` is imported as:

```rebis
(# archetypes/shadow)
```

Directories are namespaces. Adding a new `.rebis` file automatically adds a
module to runtimes that point their `ModuleResolver` at `modules/`; no central
registry or Kaos change is needed.

The first family is an original set of symbolic composition protocols inspired
by themes associated with Jung's *Archetypes and the Collective Unconscious*:
shadow, persona, inner counterpart, child, spirit, trickster, rebirth,
individuation, and mandala/wholeness. These modules are prompt structures and
composition tools, not diagnostic tests or claims about a person's psychology.

The implementations deliberately spend computation on repeated readings,
directional reversals, independent workers, explicit mediators, and bounded
continuation. Each macro has a commented source example and states its
evaluation cost. The operator guide in docs/ARCHETYPES.md explains how to
compose them without treating a prompt as hidden control flow.

The `science` family has two halves. `science/method` is about claims that
forbid an observation — hypotheses, predictions, falsification, replication.
`science/evidence` is what you do once the observations are numbers, and every
judgement in it is arithmetic in the `| |` plane and costs nothing: thresholds,
nearness to a prediction, effect size, and whether replications agree. It states
plainly what it cannot do, which is anything needing a variance.

The `forge` family is the coding repair loop: reproduce before diagnosing,
diagnose before fixing, and let something mechanical decide whether it worked.
It is also the worked example of how the two layers relate — `std/spiral` is
prompt-free and asks its caller for a frame, a distiller, a worker, and a gate,
and `forge/repair` supplies exactly those. See docs/FORGE.md.

The `science` family is about claims that forbid an observation. Every protocol
in it ends by asking what would have to be true for the claim to be wrong and
what it would cost to look; `sci-adjudicate` then hands the question to the host
through the input port, because that is the only participant that can settle
anything. See docs/SCIENCE.md.

The `git` family applies the same discipline to repository workflows rather
than Git commands. Its macros model intent, atomic change slices, durable
commit stories, adversarial review, semantic integration, conflict recovery,
release gates, and retrospective learning. See docs/GIT.md for the full API,
examples, topology, and cost model.

The `source` family is a program that is about itself. `<>` is the program as
syntax, so a program can explain, criticise, cost, or rewrite what it is — and
re-enter itself, which is a loop no macro could express. Every protocol asks
for something a compliment cannot satisfy, and none of them apply a revision:
`<>` is sight, not authority. See docs/SOURCE.md.

The `wright` family writes the program a task needs. `><` fires a prompt and
parses the answer as Rebis, and the parsed program runs — so the SHAPE of a
computation can be decided by the run rather than by whoever wrote the library.
Every gate is the host, because a model asked whether the program it just wrote
is good will say yes; and `wright-source` returns text for the cases where a
program should be read before it costs anything. See docs/WRIGHT.md.

The `conserve` family is about what a chain must not change. `@` holds a rule
across every arrow in a scope, so a stage that invents a fact, drops a
constraint, or drifts off the question is caught at the step that did it rather
than at the end. Every rule asks about a difference between two texts, never
about quality — and where a mechanical check exists, `std/amend` routes the same
question to the host for free instead. See docs/CONSERVE.md.

The `numerology` family is a second free deterministic judge. A gematria is a
pure function from text to a number, so a choice made by one is reproducible
byte for byte across runs — useful for breaking a tie, sharding an exploration,
or any pick that must survive a re-run. It claims nothing about the number: the
gate it is held to is determinism, not accuracy. See docs/NUMEROLOGY.md.

## Runtime configuration

Kaos searches for the collection in this order:

1. `REBIS_COLLECTION_PATH`, when set;
2. `../rebis-collection/modules` relative to the current working directory;
3. `../rebis-collection/modules` relative to the Kaos executable.

Point another Rebis host at the `modules/` directory and expose its files using
the host's normal module resolver.

The collection only depends on modules supplied by the Rebis standard library.
It has no runtime package, registry, generated index, or Kaos-specific API.

## Included modules

- `archetypes/core` — reusable naming, reflection, polarity, triad, and std-gate macros;
- `archetypes/shadow` — encounter and shadow-integration protocols;
- `archetypes/persona` — role, audience, mask, and projection protocols;
- `archetypes/child` — renewal, curiosity, beginning, and future-oriented work;
- `archetypes/spirit` — orientation, teaching, and meaning-making protocols;
- `archetypes/trickster` — inversion, interruption, and assumption-breaking;
- `archetypes/relations` — inner-counterpart and projection-aware dialogue;
- `archetypes/rebirth` — staged transformation and return to action;
- `archetypes/individuation` — iterative encounter and reconciliation;
- `archetypes/mandala` — center, quadrant, polarity, and std-synthesis structures.
- `git/workflow` — intent, change shape, std-review, integration, conflict,
  recovery, release, and retrospective workflow protocols; it does not wrap
  Git commands;
- `forge/repair` — reproduction, competing root causes, and a Fibonacci restart
  ladder with a host-decided gate, built on `std/spiral` and `std/seam`;
- `science/method` — hypotheses that forbid observations, cheapest
  discriminating experiments, falsification, replication, and a durable record.
