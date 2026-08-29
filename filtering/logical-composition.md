# Logical Composition

## What this document describes

This document defines how atomic filter conditions are composed into one logical expression.

It answers:

> How does ContextFilter preserve the meaning of AND / OR / nested groups / negation without flattening user intent into a lossy list?

## Implementation evidence

The delivered domain model represents a filter as a tree of `IFilterNode` objects.

`FilterGroup` contains:

```text
LogicalOperator = And | Or
Negate
IsEnabled
Children
```

and children may themselves be groups or conditions.

Therefore the system supports nested boolean structure rather than only one flat conjunction.

---

## Canonical structure

```text
ROOT GROUP
├─ CONDITION A
├─ CONDITION B
└─ GROUP C
   ├─ CONDITION C1
   └─ CONDITION C2
```

The tree itself is semantic knowledge.

```text
structure
→ grouping
→ evaluation meaning
```

Changing grouping can change the matched set even when every atomic condition remains identical.

---

## AND

For an AND group, an element must satisfy the enabled participating child conditions according to the implemented filter language.

Conceptually:

```text
A AND B
→ both constraints must hold
```

Example:

```text
Category = Walls
AND
TypeName = "200 mm"
```

is narrower than either condition alone.

---

## OR

For an OR group, satisfying one branch can be sufficient according to the implemented boolean structure.

Conceptually:

```text
A OR B
→ alternative acceptable branches
```

Example:

```text
Family = A
OR
Family = B
```

is not equivalent to an AND group containing the same two conditions.

---

## Nested groups

Nested groups allow composition such as:

```text
A
AND
(
  B
  OR
  C
)
```

This must remain different from:

```text
(A AND B)
OR
C
```

Therefore:

> **Parent-child structure is part of canonical filter intent.**

A serialization, preset, clone or optimization that preserves only the condition list but loses group boundaries changes the system meaning.

---

## Negation

Implementation evidence confirms `Negate` exists on filter nodes/groups and that group negation is supported by the client-side filter language.

Conceptually:

```text
Group result
→ optional negation
→ final group truth value
```

The exact implementation order is not duplicated here beyond what the source analysis establishes.

Important distinction:

```text
NOT (A OR B)
!=
(NOT A) OR B
```

Negation therefore cannot be treated as visual decoration.

---

## Enabled state

Implementation evidence confirms filter nodes expose `IsEnabled`.

However, the supplied analysis does not document the complete edge-case truth table for:

- empty groups;
- groups whose children are all disabled;
- interaction between disabled nodes and negation.

This repository intentionally does not invent those semantics.

The canonical requirement is:

> **Whatever the delivered implementation defines for enabled/disabled participation must be preserved consistently across evaluation, persistence, cloning and future changes.**

If the filter language evolves, these edge cases should be made explicit and verified against implementation behavior.

---

## Composition produced by quick filtering

The quick-filter use case can produce different canonical structures depending on user intent.

Implementation evidence includes:

```text
multiple selected values
→ InList
```

or, when values are requested to combine with AND:

```text
AND group
├─ Equals(value A)
└─ Equals(value B)
```

This is a useful distinction:

```text
one list condition
!= necessarily one group of multiple equality conditions
```

Even when two representations appear similar in the UI, the system should preserve the intended semantics.

---

## Composition and evaluation optimization

The implementation compiles a group tree into a linearized evaluation plan with short-circuit behavior.

That is an optimization of the same logical expression.

```text
canonical group tree
→ compile evaluation plan
→ evaluate
```

The compiled representation must not become a competing source of meaning.

Therefore:

> **Optimization may change execution order; it must not change boolean semantics.**

---

## Composition and native Revit filters

The client-side language is richer than the native Revit filter representation described in the supplied evidence.

For example, the implementation analysis identifies top-level OR groups and group negation among constructs that may not be convertible to a native Revit filter.

Therefore:

```text
valid logical composition in ContextFilter
!= guaranteed native-filter representation
```

Failure of native conversion does not make the original filter logically invalid.

---

## Persistence

Presets can persist the filter group structure.

This creates a persistence invariant:

```text
save
→ reload
→ same logical tree meaning
```

Persistence must retain more than the text labels of conditions; it must preserve grouping and logical operators sufficiently to reconstruct the original intent.

Detailed persistence ownership belongs to [`../presets/`](../presets/).

---

## Invariants

1. AND and OR are semantically different and cannot be interchanged by UI or optimization.
2. Nested group boundaries are part of canonical filter intent.
3. Negation affects meaning and must survive persistence/cloning/evaluation.
4. Execution-plan compilation may optimize order but must preserve the same logical result.
5. Native Revit incompatibility does not invalidate client-side logical structure.
6. Quick-filter authoring must compile into the same canonical group model.
7. Enabled/disabled edge cases must follow verified implementation semantics; unspecified behavior must not be guessed.

---

## Check

For any complex filter we should be able to answer:

- which conditions are siblings;
- which groups use AND and which use OR;
- where group boundaries begin and end;
- where negation applies;
- whether any nodes are disabled;
- whether serialization preserves the same structure;
- whether an optimization preserves equivalent boolean meaning;
- whether native conversion limitations affect representation only or the original filter semantics.
