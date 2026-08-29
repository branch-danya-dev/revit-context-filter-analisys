# Evaluation Contract

## What this document describes

This document defines what filter evaluation must guarantee independently from the current optimization path.

It answers:

> Given one current filterable snapshot set and one filter definition, what makes an evaluation result correct?

## Implementation evidence

The delivered `FilterEvaluator` can choose among three strategies:

```text
Inverted Index
Sequential Scan
Parallel Scan
```

The implementation analysis also describes compilation of the group tree into a linearized evaluation plan with short-circuit behavior.

These are execution strategies for one semantic contract.

---

## Canonical evaluation input

Conceptually:

```text
CURRENT FILTERABLE SNAPSHOTS
+ FILTER DEFINITION
+ required comparison knowledge
→ EVALUATION
```

Evaluation assumes upstream knowledge is usable:

- Context identifies the current candidate universe;
- Catalog provides current snapshots and sufficient parameter enrichment;
- FilterDefinition expresses canonical intent.

Filtering must not silently repair stale upstream knowledge by guessing.

---

## Canonical result

```text
FilterResult
→ identities of elements that satisfy the definition
```

The implementation representation may primarily carry element IDs and evaluation metadata, but the semantic result is simply the matched subset of the current candidate universe.

```text
result ⊆ current candidate universe
```

A result is derived state, not independent source truth.

---

## Strategy equivalence

The central invariant is:

> **For the same valid input snapshots and the same filter definition, choosing inverted-index, sequential or parallel evaluation must not change the matched element set.**

Conceptually:

```text
Evaluate_Inverted(S, D)
=
Evaluate_Sequential(S, D)
=
Evaluate_Parallel(S, D)
```

for the same semantically valid input state, when each strategy is applicable.

Performance may differ. Meaning may not.

---

## Inverted-index path

The supplied implementation analysis describes a fast path for simpler conditions such as some equality/list/existence cases.

The inverted index maps normalized parameter values to element indexes and is reused over the same snapshot list.

Semantic rule:

```text
index lookup
→ optimization of evaluation
```

not:

```text
index contents
→ new source authority
```

If index knowledge is stale relative to the snapshots/context, the result cannot be treated as current merely because lookup succeeded.

---

## Sequential scan

The sequential path evaluates the compiled condition plan over candidate snapshots.

Its semantic role is straightforward:

```text
for each candidate snapshot
→ evaluate canonical filter meaning
→ include if satisfied
```

The exact implementation ordering may be optimized, but ordering must not alter the truth value of the filter.

---

## Parallel scan

The implementation can use parallel evaluation for sufficiently large candidate sets.

Parallel execution changes scheduling, not semantics.

Therefore:

```text
parallel completion order
!= result meaning
```

The final matched set must be equivalent to sequential evaluation over the same valid input.

---

## Compiled evaluation plan

The implementation compiles the logical tree into a linearized plan with short-circuit behavior.

This compiled plan is a derived execution representation.

```text
FilterGroup tree
→ compile
→ evaluation plan
→ execute
```

The tree remains the canonical source of logical meaning.

Consequently:

1. plan compilation must preserve group structure semantics;
2. short-circuiting may avoid unnecessary work but may not change truth values;
3. a cached/compiled plan becomes stale when the definition it represents changes.

---

## Required catalog completeness

Because snapshots may be lazily enriched, evaluation must distinguish:

```text
parameter is semantically MISSING
```

from:

```text
parameter has NOT BEEN LOADED into this snapshot yet
```

Therefore authoritative evaluation requires enough Catalog knowledge for every condition being evaluated.

```text
insufficient enrichment
→ obtain required knowledge / defer evaluation
```

must not become:

```text
insufficient enrichment
→ pretend NotExists
```

---

## Currentness of result

A matched set is current only for the input state that produced it.

```text
Context C1
+ Catalog snapshot state S1
+ Definition D1
→ Result R1
```

Any relevant input change reopens the result:

```text
C1 → C2
OR S1 → S2
OR D1 → D2
→ R1 is no longer the current result
```

This remains true even if the new evaluation happens to produce the same element IDs by coincidence.

---

## Cancellation and superseded work

The implementation interface includes cancellation support, and runtime request coalescing exists elsewhere in the system.

Filtering's semantic requirement is:

> **A superseded or cancelled evaluation must not overwrite a newer authoritative result merely because it completes later.**

The concrete scheduling/coalescing mechanics remain owned by [`../runtime/`](../runtime/).

---

## Evaluation failure != empty result

A failed evaluation and a successfully evaluated filter with zero matches are different outcomes.

```text
successful evaluation
→ matched set may be empty
```

while:

```text
evaluation failure / insufficient input
!= valid empty result
```

This distinction prevents operational failures from being presented as meaningful "nothing matched" answers.

The exact user-facing failure presentation belongs to interaction/failure behavior, but Filtering owns the semantic distinction.

---

## Evaluation and actions

Evaluation stops at the matched set.

```text
FilterDefinition
→ FilterResult
```

Selection, hide, isolate, inverse isolate and native filter creation are separate actions over that result or definition.

Therefore:

```text
matched element
!= selected element
!= hidden element
!= native-filter member configuration
```

See [`../actions/`](../actions/).

---

## Evaluation and native compatibility

A filter can evaluate successfully client-side and still fail native-filter compatibility analysis.

That is not an evaluation contradiction.

```text
client evaluation succeeds
+
native conversion unsupported
→ valid ContextFilter result remains valid
```

The failure belongs to representation/action capability, not to the semantic truth of the original filter.

---

## Verification evidence

The supplied implementation analysis states that application tests cover:

- all 19 filter operators;
- AND / OR;
- negate;
- inverted-index correctness;
- quick-filter construction;
- native compatibility supported/unsupported cases.

It also states that CI does not include integration tests against a live Revit instance.

Therefore the public case can claim strong implementation-level filter-engine verification, while live-host behavior remains supported by manual/user testing rather than by automated Revit integration tests.

---

## Invariants

1. All applicable evaluation strategies must produce the same matched set for the same valid inputs.
2. The logical tree is canonical; compiled plans and indexes are derived execution representations.
3. Stale indexes/snapshots cannot become source authority.
4. Evaluation requires sufficient Catalog enrichment for referenced parameters.
5. Failure or incomplete input must not be reported as a valid empty result.
6. Changing context, catalog knowledge or filter definition reopens the previous result.
7. Parallelism may change execution order but not semantic output.
8. Client-side semantic success is independent from native Revit filter compatibility.
9. Superseded/cancelled work must not replace a newer result.

---

## Check

For any evaluation we should be able to answer:

- which candidate context it evaluated;
- which snapshot/catalog state it used;
- which exact filter definition it used;
- whether all required parameter knowledge was materialized;
- which execution strategy was chosen;
- whether another strategy would be expected to return the same matches;
- whether the result is still current;
- whether failure is being kept distinct from zero matches;
- whether later actions are using this result rather than redefining filter semantics.
