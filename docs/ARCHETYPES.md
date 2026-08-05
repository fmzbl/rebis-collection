# Archetype Modules

This guide documents the source modules in this collection. They are reusable
composition protocols inspired by images and tensions associated with Jung's
archetypal psychology. They are not personality tests, clinical instruments,
or claims that a person has a fixed archetype.

The collection is intentionally small at the repository level: the modules
directory is the library. A Rebis host only needs to make that directory
available to its normal module resolver. New files become available by
namespace; no central registry is edited.

## Rebis model

Rebis programs are data-shaped expressions. A quoted body in a macro is a
template: symbols remain syntax and comma-prefixed parameters splice caller
expressions into that template. A macro can therefore expose a short API while
constructing a much richer program.

There are two different kinds of work:

- A prompt is sent to the host's model oracle.
- A symbol, list shape, arrow, square, loop, gate, or macro expansion is
  deterministic program structure.

The distinction matters. A prompt inside a square is a branch, not an
instruction that runs after the square. A post-mediation instruction must be
an explicit downstream arrow:

    (-> ([mediator] first second)
        "Explain the std-synthesis.")

The square evaluates its branch expressions and then gives their results to its
mediator. This is the main composition primitive in these modules. It lets a
caller preserve disagreement instead of collapsing every reading into one
early answer.

An arrow flows forward. A backflow arrow written with a left arrow reads a
result back through the earlier expression. A diamond performs the same
relationship in both directions. The `%` gate `(% (judge value) yes no)`
chooses a continuation only when the judge returns the expected 1 or 0
token. Lazy gates are useful when the unchosen continuation must not run;
`[]` remains the eager mediator square.

The dollar form exposes the host input seam. The ampersand form represents
the current program or self-reference where supported by the host. These
modules use both only where a caller benefits from external context or a
self-checking pass.

## First composition

Import the module and call a public macro:

    (
      (# archetypes/core)
      (arch-name-pattern
        "Name the active pattern and keep it evidence-bound."
        "The service retries after failure but does not record attempts.")
    )

The macro creates two independent readings, mediates them, and then sends the
mediated result through a downstream constraint. The caller sees a compact
surface API, while the runtime sees the full expanded topology.

The standard library names used by the collection are ordinary Rebis modules:
canon, dialectic, flow, gate, loops, reflexion, search, and shape. They provide
the base workers and structural forms; the archetype modules add composition
protocols, not a new language layer.

## Core operators

All macros in archetypes/core are general-purpose. Their names describe the
kind of relation being constructed, not a psychological interpretation.

### arch-name-pattern lens work

Runs a lens reading and an evidence/consequence reading of the same work, then
requires a narrow downstream label.

    (arch-name-pattern
      "Name the pattern and cite the evidence."
      "The deployment rolls back manually after every failed release.")

Cost: work is evaluated in two square branches.

### arch-reflect-pattern lens work

Reads a work in its useful orientation and through its omitted counterforce.
The final arrow keeps the claim and its limit distinct.

    (arch-reflect-pattern
      "Find the excluded counterforce."
      "The migration plan optimizes speed over rollback safety.")

Cost: the work appears in both directional readings.

### arch-polarity reconcile left right

Flows two positions forward and backward before asking the reconciler what can
coexist without erasing either constraint.

    (arch-polarity
      "Reconcile the tradeoff without erasing either constraint."
      "ship quickly"
      "protect rollback safety")

Cost: both positions participate in two arrow chains.

### arch-triad reconcile first second third

Passes three independent results to a reconciler, then explicitly asks for a
downstream synthesis. The synthesis prompt is not a hidden fourth branch.

    (arch-triad
      "Mediate the three readings."
      "observable behavior"
      "operational cost"
      "next std-reversible action")

Cost: each supplied reading is an independent square branch.

### arch-round-trip worker value

Runs a worker and backflows its result into the original value. This is useful
for testing what survives a return to the starting frame.

    (arch-round-trip
      std-final-only
      "Draft a one-step rollback procedure.")

Cost: the material once, the worker twice.

### arch-lazy-gate judge accepted rejected value

Runs a binary judge and expands only the selected continuation.

    (arch-lazy-gate
      std-binary
      std-final-only
      std-one-word
      "Is this rollout safe to execute?")

Cost: the material once, the judge once, and only the selected continuation.
The material is bound before it is judged, so the branch receives exactly what
the gate ruled on.

### arch-audit-pair judge worker value

Derives the same value twice independently and gives both results to a square
mediator. The judge may be a prompt or a deterministic topic symbol.

    (arch-audit-pair
      "Choose the result that stays grounded in the evidence."
      std-with-evidence
      "Review the cache invalidation change.")

Cost: the material once, the worker twice. Both readings are of the same
material, which is what makes their agreement mean anything.

### arch-evidence-pass lens worker task

Requests separate claim, evidence, and uncertainty passes before a final
downstream account.

    (arch-evidence-pass
      "Mediate the evidence passes."
      inspect
      "Inspect the parser regression.")

Cost: the worker fires three times.

### arch-dialectic-pass reconcile left right task

Applies two caller-supplied workers, reads their relation in both directions,
and finishes with a concrete integration.

    (arch-dialectic-pass
      "Reconcile implementation and operations."
      build
      operate
      "Design the deployment boundary.")

Cost: each worker is used in both directional readings.

### arch-input-review worker judge

Reads the host input through a worker, asks a binary judge whether the result
is usable, and lazily selects a compact or explanatory continuation.

    (arch-input-review
      summarize
      std-binary)

Cost: the input worker and one selected continuation run.

### arch-converge step same value

Uses the host's bounded convergence loop: apply step, ask same whether the
result is settled, and stop when the gate returns yes.

    (arch-converge
      deepen
      std-binary
      "A draft explanation that still has unresolved contradictions.")

Cost: bounded by the standard loop and the host's loop budget.

## Archetype protocols

The remaining modules are domain-shaped arrangements of the core operators.
They do not classify people. They provide named ways to ask for a tension,
counterforce, transition, or relation while keeping evidence and uncertainty
visible.

### Shadow

shadow-scan lens work performs a surface reading, a suppressed-cost reading,
and a downstream integration. shadow-confront lens worker task compares the
ordinary reading with a worker's counter-reading. shadow-integration lens
reconcile work makes the useful and costly sides explicit. shadow-or-continue
uses a lazy judge to choose a continuation. shadow-work composes all four.
shadow-cycle repeats work under a bounded stop rule.

    (arch-shadow-work
      "Name the visible pattern and its avoided cost."
      inspect
      std-binary
      "The incident std-review omits the failed assumption.")

### Persona

role-reading lens audience work compares a public role with its private cost.
mask-check asks whether the role still fits the work. persona-revision
generates a role revision through a worker. persona-work adds a judge, and
persona-cycle repeats the role reading until the stop rule accepts it.

    (arch-persona-work
      "Read the role being performed."
      "the on-call engineer"
      revise
      std-binary
      "The team depends on one person to remember every exception.")

### Child

child-seed starts with a beginner's question and a practical first move.
child-experiment gives a worker room to propose an experiment. child-to-capacity
chooses between a compact capacity statement and a longer explanation.
child-lab combines those passes. child-growth and child-capacity provide
bounded repetition and a verification pass.

    (arch-child-lab
      "Ask what can be learned without assuming mastery."
      experiment
      std-binary
      "A new contributor is afraid to change the parser.")

### Spirit

spirit-question asks for orientation and a next meaningful action.
teachable-reading compares teacher and learner readings. spirit-with-check
places a teaching pass beside a skeptical pass. spirit-counsel adds a worker,
spirit-orientation adds verification, and spirit-cycle provides bounded return.

    (arch-spirit-orientation
      guide
      skeptic
      act
      verify
      "The project has many tools but no shared direction.")

### Trickster

trickster-opposite is a deterministic frame inversion. invert-frame backflows
an input through a caller-supplied inverter. trickster-question asks which
assumption is being protected. double-reading compares ordinary and inverted
frames. bounded-disruption gates a worker. trickster-lab combines inversion,
questioning, work, and mediation. trickster-cycle bounds repetition.

    (arch-trickster-lab
      reverse
      "Find the assumption that makes this plan look inevitable."
      challenge
      "Mediate the ordinary and inverted readings."
      "The safest plan is the one with the most approval steps.")

### Relations

counterpart and projection-check read a work through another position and
identify what the first speaker may be attributing. dialogue compares speaker
and counterpart through a reconciler. reciprocal runs a worker in both
directions. relation-work combines these readings, while relation-cycle bounds
the reciprocal process.

    (arch-relation-work
      "Read the speaker's position."
      "Read the counterpart's position."
      "Reconcile without speaking for either party."
      translate
      "The std-review says another team is careless.")

### Rebirth

threshold identifies the current crossing and a reversible next step. descent
examines the cost of entering the unknown. return checks whether a proposed
action can return with evidence. rebirth-cycle repeats those stages.
threshold-choice gates the next move, renewal-loop bounds renewal, and
rebirth-work composes the complete transition.

    (arch-rebirth-work
      "Name what must be left behind at the threshold."
      descend
      verify
      std-binary
      "The old interface prevents the new storage model.")

### Individuation

encounter keeps a lens and worker separate. reconcile mediates two readings of
one task. individuation-pass creates a claim, counterclaim, and integration.
individuation-cycle repeats that process with verification. individuate adds
the full bounded loop, and individuation-panel compares three independent
readings under a named chair.

    (arch-individuate
      "Name the tension without diagnosing the author."
      inspect
      "Reconcile both readings using only the evidence."
      std-binary
      "The code is elegant but impossible to operate.")

### Mandala

quadrant holds four directional readings under one mediator. cross-of-opposites
compares horizontal and vertical tensions. center-and-field adds a center
invariant to the four directions. mandala-review gates a field, mandala-work
adds a worker, and field-input lets the host input populate the field.

    (arch-mandala-work
      "State the invariant that must survive."
      "Mediate all four directions."
      std-binary
      map
      "The system must remain observable while changing storage.")

## Choosing an operator

Use a square when independent readings should be preserved for one mediator.
Use an arrow when order is meaningful. Use a backflow arrow when the later
reading must be tested against the earlier frame. Use a lazy gate when an
unchosen branch would be wasteful or unsafe. Use a loop only with a bounded
stop rule.

The modules intentionally repeat work. That cost buys comparison, not magic:
the host receives several independently generated views and a named place to
reconcile them. For cheap transformations, call a standard shape macro
directly. For high-stakes or ambiguous work, prefer shadow-work,
individuation-pass, relation-work, or rebirth-work and lower the host's model
budget as needed.

Source comments are the canonical examples for every macro. They sit directly
above each definition so that an editor, module browser, or generated catalog
can show the API and its cost together.

## One subject, several readings

Most figures in this family show one thing from more than one angle: a pair of
audits, four directions of a mandala, a gate and the branch it selects. That
only works if every angle is looking at the same thing — and a macro parameter
is *syntax*, so writing the subject into four positions derived it four times
and the figure lost its center.

These macros bind the subject with `=` before they fan out:

    (= reading (worker task)
       (% (judge reading)
          (field reading reading reading reading)
          (fallback reading)))

One firing, and a figure that is actually about one subject.

Two exceptions, both deliberate. `arch-mandala-review` splices on purpose: its
field wants four *independent* readings, and two of its quarters are inverted
frames. And inversion is why: `^` transforms **syntax**, so `(^ n)` for a bound
name inverts nothing and silently returns the answer unchanged. Invert the
expression and bind the result — `(= n (^ (work task)) …)` — never the other way
around.

## Carrying work between sessions

`arch-integrated subject work` does the work with everything the project already
knows about the subject, and keeps what came out:

    (arch-integrated the-migration arch-hold-the-tension)

Individuation is the one figure here explicitly about carrying something
forward, and until a run could keep an answer past its own end it could not be
expressed — every session re-derived the same integration from nothing.
