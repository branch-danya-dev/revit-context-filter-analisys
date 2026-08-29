# Filter Definition

## What this document describes

This document defines the canonical **filter intent** used by ContextFilter.

It answers:

> What information must survive after UI interaction is translated into a filter that can be evaluated, stored or later adapted to another representation?

## What it intentionally does not describe

It does not define:

- how Revit elements enter the current context;
- how parameter values are extracted from Revit;
- which evaluation strategy is fastest;
- how the result is selected, hidden or isolated;
- how a native Revit filter is created.

Those concerns belong to neighboring responsibility areas.

---

## Implementation evidence

The delivered model contains a `FilterDefinition` with:

```text
Scope
RootGroup
SelectedCategoryKeys
```

The root group contains nested `FilterGroup` and `FilterCondition` nodes.

A `FilterCondition` contains:

```text
ParameterKey
FilterOperator
Operands
IgnoreCase
```

This gives the system an explicit semantic representation between UI interaction and evaluation.

---

## Canonical model

```text
USER INTENT
    ↓ authoring
FILTER DEFINITION
    ├─ source-scope intent
    ├─ category restriction evidence
    └─ logical condition tree
            ↓
       FILTER EVALUATION
```

The filter definition is the canonical description of **what should match**, not of **how the evaluator should find matches**.

---

## Filter definition != UI state

The UI can present the same intent in many ways:

- selected tree nodes;
- parameter/value checkboxes;
- active-condition chips;
- a preset;
- a template completed with project-specific values.

Those are authoring and presentation mechanisms.

```text
UI controls
→ translate
→ FilterDefinition
```

Therefore:

> **UI representation is evidence of intent; `FilterDefinition` is the semantic contract consumed by evaluation.**

---

## Scope inside the definition

The implementation stores `CollectionScope` in the filter definition.

This does not move Context ownership into Filtering.

The distinction is:

```text
Context
→ owns the actual current candidate universe

FilterDefinition.Scope
→ records which scope the intent expects
```

Filtering must not silently evaluate a definition against an unrelated candidate universe and call the result equivalent.

The exact compatibility/recollection behavior between a stored definition and a newly current context belongs to cross-area workflow analysis.

---

## Selected categories

The implementation stores `SelectedCategoryKeys` separately from the logical root group.

This establishes that category selection is part of filter intent, but the supplied implementation analysis does not fully specify every internal ordering rule between category narrowing and root-group evaluation.

The safe canonical statement is:

> **Category selection constrains the intended candidate population and must be preserved when the filter is reconstructed.**

This repository does not invent a more detailed execution order where source evidence does not establish one.

---

## Atomic condition

Conceptually:

```text
FILTER CONDITION
=
Parameter identity
+ operator
+ operands
+ comparison policy
```

### Parameter identity

The condition references a canonical [`ParameterKey`](../catalog/parameter-identity.md), not merely the visible parameter name.

```text
"Марка"
!= sufficient canonical reference
```

### Operator

The operator determines what relationship must hold between source value state and operands.

See [`operator-semantics.md`](operator-semantics.md).

### Operands

Operands contain the comparison input required by the selected operator.

The exact arity depends on the operator family—for example equality, list or range operations have different needs.

### Comparison policy

The implementation includes `IgnoreCase` on a condition. This is part of semantic comparison behavior, not merely a rendering option.

---

## Quick filter compilation

Implementation evidence shows that the quick-filter UI compiles selected catalog values into the canonical definition.

Examples:

```text
special missing token
→ NotExists

special empty token
→ IsEmpty

single concrete value
→ Equals

multiple concrete values
→ InList
   OR a composed AND group when requested
```

This leads to an important rule:

> **Convenient authoring must not create a hidden second filter language.**

A quick filter, preset and manually composed filter should converge on the same semantic model before evaluation.

---

## Reusable intent

Because presets can store the filter group and scope, the filter definition is also the bridge between transient UI interaction and reusable intent.

However:

```text
FilterDefinition
!= persisted preset document
```

Persistence metadata, schema versioning, names and template/full distinction belong to [`../presets/`](../presets/).

---

## Definition validity

A filter definition may be structurally valid while requiring catalog knowledge that is not currently materialized.

For example:

```text
valid ParameterKey in condition
+ current light snapshots without that parameter loaded
```

must not be interpreted as:

```text
parameter NotExists for every element
```

Catalog enrichment must provide sufficient knowledge before authoritative evaluation.

See [`../catalog/snapshot-contract.md`](../catalog/snapshot-contract.md).

---

## Definition change and result freshness

A result is derived from a particular definition state.

```text
FilterDefinition D1
+ Current snapshots S1
→ Result R1

change D1 → D2
→ R1 is no longer the current result for D2
```

Even if the context did not change, changing the filter intent reopens the result.

---

## Invariants

1. The definition captures semantic intent independently from evaluation optimization.
2. Conditions reference canonical parameter identity, not display labels.
3. Logical structure is preserved as part of the definition.
4. Quick-filter UI compiles into the same canonical model.
5. Definition scope records intent but does not become source-context authority.
6. Category restrictions are preserved independently from UI presentation.
7. A definition change invalidates the previous derived matched set.
8. A structurally valid definition still requires sufficient current catalog knowledge for correct evaluation.

---

## Check

For any filter definition we should be able to answer:

- which scope it expects;
- which category restrictions it carries;
- what its root logical structure is;
- which canonical parameter identities it references;
- which operators and operands are required;
- which comparison policies affect meaning;
- whether current Catalog knowledge is sufficient to evaluate it;
- which previous result must be reopened when the definition changes.
