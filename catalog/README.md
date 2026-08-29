# Catalog

This area owns **how source Revit elements are projected into filterable categories, families, types, parameters and values**.

## Canonical question

> What filterable knowledge can the user see about the current context without confusing presentation labels with canonical identity?

## Context tree

The current system exposes a three-level navigation projection:

```text
Category
  └─ Family
       └─ Type
```

Each node represents a derived grouping over source element IDs. The tree helps narrow the working set; it is not itself source model ownership.

## Parameter identity

A parameter cannot be identified safely only by its visible name.

Conceptually the system distinguishes:

```text
identity kind
+ identity value
+ source
```

where source includes instance, type and synthetic values.

Synthetic filterable properties include concepts such as:

- Category;
- Family;
- TypeName;
- ElementId / UniqueId;
- Workset;
- Level.

They are derived during snapshot construction rather than treated as native Revit parameters.

## Values

Parameter values are projected into normalized filter values while retaining the distinction between:

```text
parameter absent
!= parameter exists but empty
!= parameter has concrete value
```

This distinction is essential for filter correctness.

## Lazy projection

The implementation may initially collect lightweight identity/classification data and load deeper parameter data only when needed. This is an optimization of the same semantic catalog, not a different model.

## Ownership

- Revit owns source element/parameter data.
- ContextFilter owns the derived catalog and parameter identity mapping used by filtering.
- Display names are presentation evidence, not canonical identity.

## Does not own

- which elements enter the context → [`../context/`](../context/);
- what operators mean → [`../filtering/`](../filtering/);
- Revit-native parameter resolution → [`../revit-boundary/`](../revit-boundary/).
