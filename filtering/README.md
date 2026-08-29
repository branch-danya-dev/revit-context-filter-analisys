# Filtering

This area owns **what a ContextFilter condition means and how a filter result is derived from one current context**.

## Canonical question

> Given a bounded context and filter intent, which elements satisfy that intent?

## Filter model

The implemented filter is a logical tree rather than a flat list:

```text
FilterDefinition
├─ selected categories
└─ RootGroup
   ├─ AND / OR
   ├─ optional negate
   └─ conditions / nested groups
```

An atomic condition combines:

```text
Parameter identity
+ operator
+ operands
+ comparison policy
```

## Semantic operators

The implementation supports equality, string, existence/emptiness, numeric, range and list operations.

Important distinctions include:

```text
Equals
!= Contains

NotExists
!= IsEmpty

AND
!= OR
```

The original requirements explicitly exposed a correctness problem in a reference workflow where filtering by a type value could also select unrelated types. ContextFilter therefore treats operator semantics as a correctness concern rather than as UI text.

## Evaluation

Filtering is evaluated over in-memory element snapshots rather than by repeatedly issuing Revit API queries for every condition.

The implementation may choose among inverted-index, sequential and parallel evaluation strategies. These strategies are equivalent realizations of one semantic contract:

> **For the same fresh context and filter definition, evaluation strategy must not change the matched set.**

## Result

```text
FreshContext
+ FilterDefinition
→ FilterResult(ElementIds)
```

`FilterResult` is derived state. It is not authoritative after the context or filter definition changes.

## Native-filter boundary

A semantically valid filter does not have to be convertible into a native Revit filter. Native compatibility is a separate realization question owned jointly by [`../actions/`](../actions/) and [`../revit-boundary/`](../revit-boundary/).

## Does not own

- source parameter identity → [`../catalog/`](../catalog/);
- what happens to matches → [`../actions/`](../actions/);
- performance thresholds → [`../runtime/`](../runtime/).
