# Parameter Value Semantics

## What this document describes

This document defines **what Catalog means when it exposes values for a filterable parameter**.

It answers:

> What is the difference between a parameter that is absent, a parameter that exists but has no value, and a parameter with a concrete value?

## What it intentionally does not describe

It does not define the full semantics of operators such as `Equals`, `Contains`, `Between`, AND or OR. Those belong to [`../filtering/`](../filtering/).

It also does not define Revit storage conversion mechanics, which belong to [`../revit-boundary/`](../revit-boundary/).

## Source evidence

Current implementation evidence provides several important facts:

- `ParameterValueKind` distinguishes typed values such as string, integer, double, boolean and element ID;
- `BuildParameterValuesUseCase` produces unique values for a selected parameter;
- the quick-filter builder recognizes two special UI/value tokens:
  - `__missing__` → semantic `NotExists`;
  - `__empty__` → semantic `IsEmpty`;
- a `ParameterValueNormalizer` normalizes strings and numbers for filtering/indexing;
- `RevitParameterValueConverter` converts native Revit values into the domain representation.

These implementation details support a stronger semantic distinction than a simple string list.

## Canonical value states

For a requested parameter identity and one source element, Catalog must preserve at least the following semantic states:

```text
MISSING
parameter does not exist for the element

EMPTY
parameter exists, but its filterable value is empty

CONCRETE
parameter exists and has a filterable typed value
```

Therefore:

```text
MISSING
!= EMPTY
!= ""
!= 0
!= false
```

A concrete zero/false value must not be collapsed into absence merely because it is visually or logically “empty-like”.

## Missing vs empty

This distinction is already observable in the delivered quick-filter behavior:

```text
__missing__
→ NotExists

__empty__
→ IsEmpty
```

If Catalog collapsed both states into one value before Filtering received them, the filter engine could no longer implement these two different user intents correctly.

Canonical rule:

> **Catalog preserves source-value state; Filtering decides what an operator means over that state.**

## Not loaded is a fourth concern

Because snapshots are lazily enriched, there is another representation state:

```text
NOT LOADED
```

This is not a parameter value state.

It means the current derived snapshot does not yet contain enough knowledge to decide whether the source parameter is missing, empty or concrete.

```text
NOT LOADED
!= MISSING
!= EMPTY
```

See [`snapshot-contract.md`](snapshot-contract.md).

## Typed values

The implementation uses a `ParameterValueKind` rather than flattening every value to display text.

Current evidence names kinds including:

```text
String
Integer
Double
Boolean
ElementId
...
```

The repository intentionally does not invent the complete enum beyond the supplied evidence.

The semantic consequence is:

> **A display string is not automatically the canonical value representation.**

For example, numeric filter behavior depends on preserving numeric meaning rather than comparing formatted strings.

## Source value → filterable value

Conceptually:

```text
REVIT SOURCE VALUE
        ↓
Revit value conversion
        ↓
TYPED PARAMETER VALUE
        ↓
normalization / index representation
        ↓
FILTER EVALUATION
```

The converter and normalizer are implementation mechanisms. Catalog owns the requirement that the resulting representation preserve the meaning required by filtering.

## Normalization

Implementation evidence includes normalization of strings (`trim`, case handling) and numbers.

Normalization must be understood as:

```text
source value
→ canonical comparison representation
```

not:

```text
source value
→ silently redefine source fact
```

For example, case-insensitive comparison may normalize strings for evaluation while the original display text can remain different.

The exact comparison rule is owned by Filtering.

## Unique value discovery

The UI can request unique values for a selected `ParameterKey`.

Semantically:

```text
current candidate subset
+ selected ParameterKey
        ↓
Catalog value projection
        ↓
unique filterable choices
```

This value list is derived from the current context/catalog state. It is not a global dictionary of all possible values in Revit.

## Scope dependency

The same parameter identity may expose different available values under different Context scopes.

```text
ActiveView
→ values visible in that candidate universe

CurrentSelection
→ values represented in selected elements

EntireDocument
→ values represented in document candidate universe
```

Therefore:

```text
same ParameterKey
+ different Context
→ potentially different value catalog
```

The parameter identity can remain stable while its current value population changes.

## Synthetic values

Synthetic parameters participate in the same value surface even though their values are derived from different source facts.

Examples from current implementation evidence include category/family/type names, element IDs, workset and level.

```text
synthetic source rule
→ typed / filterable value
→ same Catalog value semantics
```

Again:

```text
same value interface
!= same native source representation
```

## Display values vs comparison values

The user needs readable values; the filter engine needs stable comparison values.

These concerns may coincide physically, but they are conceptually different:

```text
DISPLAY VALUE
→ what the user reads

FILTERABLE VALUE
→ what semantic evaluation consumes
```

Any implementation change that introduces formatting/localization must preserve this distinction.

## Freshness

Unique values, indexes and normalized representations are derived from snapshots.

If the relevant Context or snapshots become stale:

```text
old value catalog
→ derived from old source evidence
→ must not be treated as current available-value truth
```

Runtime may cache these projections, but cache storage does not change their authority.

## Invariants

1. Missing and empty are different semantic states.
2. Not-loaded is not evidence of missing or empty.
3. Concrete zero/false values are not absence.
4. Parameter values retain enough type meaning for correct filtering.
5. Normalization supports comparison; it does not become source authority.
6. Unique value lists are derived from the current bounded candidate population.
7. Same parameter identity may have different current value populations in different contexts.
8. Display representation and comparison representation must not be confused.

## Typical analytical mistakes

### `null`, empty string and missing are all the same

The delivered system already contradicts this by supporting separate existence and emptiness operators.

### Everything becomes a string

That can destroy numeric/boolean semantics and make range/comparison behavior incorrect.

### Parameter not loaded means parameter absent

Lazy snapshots make this false.

### Unique values are global metadata

They are derived from the current candidate set, so their population depends on Context.

## Check

For any value shown in Catalog we should be able to answer:

- which `ParameterKey` it belongs to;
- which current candidate population produced it;
- whether the parameter was missing, empty or concrete on a given element;
- what typed meaning the value retains;
- what normalization was applied for comparison;
- whether the representation is current enough for filtering.
