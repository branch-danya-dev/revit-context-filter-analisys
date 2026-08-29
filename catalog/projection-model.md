# Catalog Projection Model

## What this document describes

This document defines the **derived classification projection** used to navigate the current ContextFilter candidate set.

It answers:

> How does a flat set of candidate Revit elements become navigable as Category → Family → Type without changing who owns the source elements?

## What it intentionally does not describe

It does not define:

- which elements belong to the current session — [`../context/`](../context/);
- parameter identity — [`parameter-identity.md`](parameter-identity.md);
- filter condition semantics — [`../filtering/`](../filtering/);
- WPF tree controls or virtualization details.

## Source evidence

Current implementation evidence contains:

- `ElementTreeRecord` as source data for tree construction;
- `BuildContextTreeUseCase` for building `Category → Family → Type`;
- `ContextNode` with `ElementIds`, `TotalCount`, `IsChecked`, `DisplayName` and `Key`;
- the three node kinds `Category`, `Family`, `Type`.

The physical DTOs are implementation evidence. The canonical knowledge here is the semantic projection they realize.

## Projection

```text
CURRENT CANDIDATE ELEMENTS
          ↓
      CLASSIFY
          ↓
      CATEGORY
          ↓
       FAMILY
          ↓
        TYPE
```

The projection exists to answer navigation questions such as:

- Which categories are present in this context?
- Which families are represented under a category?
- Which types are represented under a family?
- Which source element IDs belong to a selected branch of this projection?

## Derived knowledge

The tree is not an independent source of BIM truth.

```text
Revit elements
→ current Context candidate set
→ classification records
→ Catalog tree
```

Therefore:

```text
Catalog node
!= Revit element

Catalog hierarchy
!= Revit ownership hierarchy

node count
!= independent source fact
```

Node membership and counts are derived from the current candidate set.

## Projection ownership

Catalog owns the current classification projection.

Revit remains authoritative for the source element facts from which classification is derived.

Context remains authoritative inside ContextFilter for **which candidate element IDs belong to this session**.

This gives the boundary:

```text
REVIT
owns source facts

CONTEXT
owns bounded candidate universe

CATALOG
owns derived navigation projection
```

## Why the projection is not Context ownership

The current implementation may physically return `TreeRoots` and `ElementTreeRecord` data alongside context IDs in one `CollectedContext` structure.

That physical co-location does not collapse responsibilities.

The semantic questions remain different:

```text
Context:
Which elements are in scope?

Catalog:
How are those elements exposed as navigable filterable knowledge?
```

This distinction matters when one representation changes without changing the other. A different UI grouping could be introduced while the same context universe remains valid.

## Selection of tree branches

Implementation evidence includes a `ContextTreeSelectionService` that calculates element IDs from checked tree nodes.

Semantically:

```text
checked catalog nodes
→ derived subset of current context IDs
```

The result is a **working subset**, not a newly authoritative context source.

A Catalog selection cannot add source elements that are outside the current Context candidate universe.

## Display name and key

A tree node has both a display name and a key in the implementation.

The same principle used for parameters applies here:

> **Presentation text is not automatically canonical identity.**

The repository does not infer stronger category/family/type identity guarantees than the supplied implementation evidence establishes. If identity rules for these nodes change, this model must be reopened.

## Freshness

The projection is meaningful only for the candidate universe from which it was built.

```text
Context C1
→ Catalog Projection P1

Context changes to C2
→ P1 is derived from old evidence
→ P1 must not be treated as the current catalog
```

Whether Context can be incrementally patched or must be fully rebuilt is owned by [`../context/freshness-lifecycle.md`](../context/freshness-lifecycle.md) and runtime realization. Catalog owns the consequence: its projection must correspond to the current context.

## Invariants

1. Every catalog projection is derived from one bounded context.
2. The projection cannot expand beyond the context candidate set.
3. Category → Family → Type is navigation knowledge, not source ownership.
4. UI labels do not become source authority.
5. A stale context implies that its derived catalog projection cannot be treated as current.
6. Changing presentation/grouping does not automatically redefine Context scope.

## Typical analytical mistakes

### Treating the tree as the model itself

```text
Wrong:
TreeView = system model
```

The tree is one representation of derived catalog knowledge.

### Letting UI state define source membership

A checked node describes current user narrowing over the catalog. It does not change which elements exist in Revit.

### Mixing Context and Catalog

A candidate set answers **where we search**. A classification tree answers **how we navigate what was found there**.

## Check

The projection model is coherent when we can answer separately:

- what candidate universe produced this tree;
- what each tree level represents;
- which source IDs a branch corresponds to;
- why a label is not automatically identity;
- what happens to the projection when its source context becomes stale.
