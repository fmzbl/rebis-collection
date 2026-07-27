
# Git Workflow Module

`git/workflow` is a Rebis module for the reasoning around repository changes.
It does not reimplement `git status`, `git merge`, `git commit`, or any other
repository command. A host may use those commands to gather evidence, but this
module decides how that evidence is interpreted, challenged, integrated, and
carried forward.

Import it with:

```rebis
(# git/workflow)
```

The source lives at `modules/git/workflow.rebis`. Collection modules are
definition-only: importing the module installs its macros; it does not start a
model call. A host can resolve the file directly from the collection path.

## Workflow model

The module treats repository work as a change line with six durable concerns:

1. **Intent** — what outcome and invariant justify the change?
2. **Shape** — what are the smallest independently verifiable slices?
3. **Story** — what should one durable commit or review unit preserve?
4. **Challenge** — which independent reading could falsify the proposal?
5. **Integration** — how are two valid intentions reconciled without silently
   replacing one with the other?
6. **Recovery and learning** — what is the least irreversible safe action, and
   what should the next change inherit?

These are abstractions over decisions, not abstractions over shell syntax. A
caller can pass a prompt, an arrow pipeline, a mediator square, an input port,
or another macro as an argument. This makes the module useful in an agent that
already has repository tools as well as in a host that only supplies written
evidence.

## Rebis topology

The implementations intentionally use two different structures:

- `->` routes the actual answer from one stage into the next stage's `INPUT:`.
- `(<- consumer producer)` writes the same relationship from the consumer
  side.
- `([judge] a b c)` evaluates independent branches and then gives their
  results to the mediator. The branches do not implicitly feed one another.
- `(% (check value) one zero)` is the explicit lazy binary form. The check
  must answer exactly `1` or `0`; only the selected continuation expands.
  `[]` remains eager mediation.
- `'(...)` makes a macro template. `,argument` splices the caller's complete
  syntax tree into that template.

The source keeps the final deliverable prompt **outside** a mediation square.
That matters: a prompt written after the mediator inside `(...)` would be
another independent branch, not a post-mediation stage. Every public macro in
this module makes the intended post-mediation stage explicit with an arrow.

## API

All names are prefixed with `git-` so the module can be imported beside other
collection families without collisions.

### `git-intent`

```rebis
(git-intent frame task)
```

Builds an intent contract containing the user-visible outcome, invariants,
affected surface, evidence, and a non-goal. `frame` is a mediator prompt or a
one-argument judge macro. `task` can be any Rebis expression.

```rebis
(git-intent
  "Reconcile the intent statements without flattening constraints."
  "Add a safe workflow for changing the collection resolver.")
```

Four readings are reconciled before the final contract is written. This is the
right entry point when a request is still ambiguous.

### `git-slice-change`

```rebis
(git-slice-change judge task)
```

Finds atomic, independently verifiable change slices and their dependency
order. It distinguishes behavior, refactoring, generated output,
documentation, and migration boundaries.

```rebis
(git-slice-change
  "Choose the smallest safe slices and preserve their dependencies."
  "Separate the parser, terminal, visual, and documentation work.")
```

The output is a change map, not a sequence of repository commands.

### `git-change-plan`

```rebis
(git-change-plan judge task)
```

Runs `git-intent` first, then sends its result into a second independent
planning square. The plan includes implementation order, falsification
evidence, compatibility risks, ownership, verification, and rollback criteria.

```rebis
(git-change-plan
  "Select the plan that protects the strongest invariant."
  "Add a source-only module that Kaos and other Rebis hosts can resolve.")
```

Use this instead of asking an agent for a patch plan with no explicit contract.

### `git-commit-story`

```rebis
(git-commit-story judge task)
```

Shapes a durable, single-intention story for a commit or review unit. It
records the before/after behavior, invariant, evidence, verification, and
rollback boundary. The result is useful after the original conversation has
ended.

```rebis
(git-commit-story
  "Reject mixed-intention stories."
  "The run browser needs a chat entry point for a completed run.")
```

The answer is passed through `std/shape/std-with-evidence`.

### `git-review-cycle`

```rebis
(git-review-cycle judge worker critic repairer task)
```

Runs a worker through a critic and repairer, keeps an independent adversarial
reading beside that path, and asks for verification evidence. `worker`,
`critic`, and `repairer` are one-argument macro names or higher-order
expressions. `judge` is the mediator.

```rebis
(git-review-cycle
  "Prefer the repair that fixes the root cause with the least new risk."
  implement
  attack
  repair
  "Add a resolver for source-only collection modules.")
```

The original path is not discarded when the repair is produced. That makes a
reviewer's disagreement inspectable rather than allowing the last answer to
masquerade as consensus.

### `git-review-loop`

```rebis
(git-review-loop judge worker critic repairer stop value)
```

Repeats `git-review-cycle` until the one-argument `stop` macro answers exactly
`yes`. The `%` gate leaves the accepted value untouched and never
expands the recursive branch after convergence.

```rebis
(git-review-loop
  "Choose the safer reviewed result."
  implement
  attack
  repair
  settled
  "Draft the migration plan.")
```

The runtime's macro-expansion and model-call limits are the hard bound. Keep
`stop` cheap and deterministic enough to make the budget meaningful.

### `git-reconcile-lines`

```rebis
(git-reconcile-lines judge ours theirs)
```

Compares two lines of work by extracting each line's intention, invariant,
evidence, and irreducible value. It also runs a cross-line comparison in both
written orientations before producing an integration decision.

```rebis
(git-reconcile-lines
  "Preserve both valid intentions and expose every tradeoff."
  "The local line adds live run chat."
  "The incoming line adds finished-run transcript recovery.")
```

The answer explicitly chooses among preserve, combine, replace, and defer. It
also names unresolved evidence and the owner of the next step.

### `git-conflict-resolution`

```rebis
(git-conflict-resolution judge resolver verifier conflict)
```

Classifies a conflict, reconstructs the invariant on each side, sends a
candidate resolution through a verifier, and compares rollback, defer, and
preserve-both options. `resolver` and `verifier` are one-argument macro names.

```rebis
(git-conflict-resolution
  "Prefer a behavior-preserving resolution with visible uncertainty."
  resolve-conflict
  verify-resolution
  "Two implementations disagree about whether an unknown module is fatal.")
```

This is semantic conflict work. It does not assume that a textual conflict
marker identifies the real boundary of disagreement.

### `git-history-inquiry`

```rebis
(git-history-inquiry judge question)
```

Reads repository history as evidence for intent and precedent. It distinguishes
observed history from inference, compares old assumptions with the present
architecture, and runs a current-day counterfactual.

```rebis
(git-history-inquiry
  "Weigh precedent against present evidence; age is not authority."
  "Why does this boundary reject unknown collection namespaces?")
```

A host can make `question` a program that gathers historical material, so the
module remains independent of a particular Git client or log format.

### `git-stack-integrity`

```rebis
(git-stack-integrity judge series)
```

Reviews a sequence of changes as a coherent argument. It examines every
partial state, reversibility, dependency order, duplicated intent, hidden
coupling, and the reviewer walkthrough.

```rebis
(git-stack-integrity
  "Prefer a series understandable and safe at every boundary."
  "A four-step series separates parser, runtime, terminal, and visual changes.")
```

`series` need not be a list of commit identifiers; it may be generated std-review
evidence or another Rebis program.

### `git-release-gate`

```rebis
(git-release-gate std-gate candidate)
```

Runs a lazy release decision. `std-gate` is a one-argument macro that returns
exactly `1` or `0`; only the accepted or rejected continuation expands.

```rebis
(~ ready (candidate)
  '(-> ,candidate "Is the candidate ready? Answer exactly 0 or 1."))

(git-release-gate ready "The reviewed change candidate")
```

The accepted branch records monitored risk and rollback triggers. The rejected
branch records blocking evidence and the smallest repair needed to reopen the
gate.

### `git-recovery-plan`

```rebis
(git-recovery-plan judge incident)
```

Builds an evidence-first recovery brief: timeline, broken invariant,
containment, rollback versus forward-fix, verification probes, communication,
ownership, and follow-up learning.

```rebis
(git-recovery-plan
  "Choose the least irreversible action that restores the invariant."
  "A release introduced intermittent failures after a repository split.")
```

It treats rollback as one candidate among several, not as a reflex.

### `git-retrospective`

```rebis
(git-retrospective judge outcome)
```

Compares intended and observed outcomes, identifies predictive and noisy
signals, separates process constraints from blame, and produces one owned,
measurable workflow change.

```rebis
(git-retrospective
  "Keep lessons that change future decisions and discard unsupported blame."
  "The module shipped after two review rounds and one delayed rollback test.")
```

## Composition example

The collection stays small at the import boundary while allowing a host to
compose a larger workflow. This example is still ordinary Rebis: the arrows
carry actual answers, and the square branches remain independent until their
mediator.

```rebis
(
  (# git/workflow)

  (~ worker (task)
    '(-> ,task "Implement one smallest change slice and state its evidence."))

  (~ critic (task)
    '(-> ,task "Attack this change for regressions and missing invariants."))

  (~ repairer (task)
    '(-> ,task "Repair only the demonstrated defect and preserve the invariant."))

  (~ settled (value)
    '(-> ,value "Is the evidence sufficient to accept this std-review? Answer exactly 0 or 1."))

  (git-review-loop
    "Select the safest evidence-backed result."
    worker
    critic
    repairer
    settled
    (git-change-plan
      "Reconcile the intent readings."
      "Add an auditable Git workflow module to the Rebis collection.")))
```

The collection does not prescribe how a host obtains repository evidence. A
Kaos run may provide source and tool output through prompts or input ports; a
different runtime may provide the same material through its own resolver and
oracle. The workflow macros remain the same.

## Cost and safety

These macros deliberately duplicate some task expressions. Rebis has no hidden
variable binding for model answers: an answer reaches a later stage through an
arrow. Repetition therefore means independent evidence, not an accidental
optimization failure. Use the lightest macro that preserves the uncertainty
you need, and set runtime model, expansion, and concurrency limits in the host.

All public names carry the `git-` prefix. Adding another file under
`modules/git/` creates another namespace module without changing this file or
Kaos.
