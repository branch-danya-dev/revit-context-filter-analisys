# Filtering

This area owns **the semantic meaning of a ContextFilter filter and the derivation of one matched element set from current catalog knowledge**.

## Canonical question

> Given one current candidate context and one filter definition, which elements satisfy the user's intent?

Filtering begins only after Context and Catalog have established a usable source universe and filterable representation.

```text
CURRENT CONTEXT
      ↓
CURRENT FILTERABLE SNAPSHOTS
      ↓
FILTER DEFINITION
      ↓
SEMANTIC EVALUATION
      ↓
MATCHED ELEMENT SET
```

The matched set is derived knowledge. It is not authoritative independently from the context, catalog state and filter definition that produced it.

---

## Canonical models

| Document | Owns |
|---|---|
| [`filter-definition.md`](filter-definition.md) | What information constitutes filter intent and what is merely UI representation |
| [`logical-composition.md`](logical-composition.md) | Nested groups, AND / OR, negation and condition composition |
| [`operator-semantics.md`](operator-semantics.md) | The supported operator families and the distinctions they preserve |
| [`evaluation-contract.md`](evaluation-contract.md) | What evaluation must guarantee regardless of inverted / sequential / parallel execution |

---

## Core distinction

```text
FILTER MEANING
!= EVALUATION STRATEGY
!= NATIVE REVIT REPRESENTATION
```

A filter can be semantically valid even when:

- the evaluator chooses a different optimization path;
- the filter cannot be represented as a native Revit `ElementFilter` / `ParameterFilterElement`;
- the UI that created it is no longer open.

This separation is one of the central system boundaries in the case.

---

## Filter definition

The implemented model is a logical tree rather than a flat list.

```text
FilterDefinition
├─ Scope
├─ SelectedCategoryKeys
└─ RootGroup
   ├─ FilterCondition
   └─ FilterGroup
      └─ nested nodes
```

An atomic condition carries:

```text
ParameterKey
+ FilterOperator
+ Operands
+ comparison policy such as IgnoreCase
```

See [`filter-definition.md`](filter-definition.md).

---

## Logical composition

The implementation explicitly supports:

```text
AND
OR
Negate
nested groups
```

Therefore a filter is not equivalent to a list of independent checkboxes. Group structure is part of canonical intent.

```text
(A AND B)
!=
(A OR B)

NOT (A OR B)
!=
(NOT A) OR B
```

See [`logical-composition.md`](logical-composition.md).

---

## Operator families

The implementation evidence lists 19 operators across these families:

- equality;
- string matching;
- existence / emptiness;
- numeric comparisons;
- ranges;
- lists.

Important distinctions include:

```text
Equals
!= Contains

NotExists
!= IsEmpty

Between
!= InList
```

Missing and empty value semantics are inherited from [`../catalog/value-semantics.md`](../catalog/value-semantics.md); Filtering owns what operators do with those states.

See [`operator-semantics.md`](operator-semantics.md).

---

## Quick filter is an authoring path, not a second filter model

Implementation evidence shows that UI value selection is converted into the same canonical `FilterDefinition` used by the evaluator.

Examples from the delivered behavior:

```text
__missing__
→ NotExists

__empty__
→ IsEmpty

one selected value
→ Equals

multiple selected values
→ InList
   OR an AND group of Equals when requested
```

Therefore:

> **Quick filtering is a way to construct filter intent; it does not own separate matching semantics.**

---

## Evaluation

Filtering is evaluated over in-memory `ElementSnapshot` knowledge rather than by repeatedly executing every semantic condition through direct Revit API access.

The delivered evaluator may choose:

```text
Inverted index fast path
Sequential scan
Parallel scan
```

These are implementation strategies for one contract:

> **For the same current input knowledge and filter definition, evaluation strategy must not change which elements match.**

See [`evaluation-contract.md`](evaluation-contract.md).

---

## Result ownership

Conceptually:

```text
FilterResult
=
matched source element identities
+ provenance of the filter/context state that produced them
```

The implementation representation may primarily carry element IDs, but semantically the result is valid only while its inputs remain valid.

```text
context changed
OR catalog knowledge changed
OR filter definition changed
        ↓
previous filter result is no longer current
```

Filtering owns the matched-set derivation, not the later action performed on that set.

---

## Native Revit boundary

Implementation evidence explicitly shows that native Revit filter conversion supports only a subset of the semantic filter language.

Examples of unsupported or restricted constructs in the supplied analysis include:

- group negation;
- several string/range operators;
- most synthetic parameters;
- top-level OR groups.

Therefore:

```text
semantically valid ContextFilter filter
!= necessarily native-Revit-compatible filter
```

Compatibility analysis is consumed here as a boundary fact, while native creation and Revit realization remain owned by [`../actions/`](../actions/) and [`../revit-boundary/`](../revit-boundary/).

---

## Ownership

Filtering owns:

- the canonical filter definition;
- logical composition of conditions;
- operator meaning;
- semantic evaluation over current filterable knowledge;
- the derived matched element set;
- equivalence of evaluation strategies.

Filtering does **not** own:

- which Revit elements belong to the search universe → [`../context/`](../context/);
- parameter identity and value-state representation → [`../catalog/`](../catalog/);
- what the user does with matches → [`../actions/`](../actions/);
- persistence of reusable filters → [`../presets/`](../presets/);
- thresholds, parallelism policy or cache mechanics → [`../runtime/`](../runtime/);
- native parameter resolution / Revit transactions → [`../revit-boundary/`](../revit-boundary/).

---

## Key invariants

1. Filter meaning is independent from the current evaluator optimization path.
2. Logical group structure is part of intent and must not be flattened without preserving semantics.
3. `NotExists` and `IsEmpty` remain different because Catalog exposes different source states.
4. `ParameterKey`, not the display label, identifies the parameter referenced by a condition.
5. A filter result is derived from a particular current input state and becomes stale when those inputs change.
6. Native Revit compatibility is not a prerequisite for semantic validity.
7. UI convenience constructs must compile into the canonical filter model rather than introduce hidden matching rules.

---

## Evidence boundary

The supplied implementation analysis confirms that filter nodes include `IsEnabled` and `Negate`. It does not explicitly document the complete truth table for every edge case involving empty or disabled groups.

This repository therefore does not invent those edge-case semantics. They should be treated as implementation behavior that must remain verified if the filter language is changed.
