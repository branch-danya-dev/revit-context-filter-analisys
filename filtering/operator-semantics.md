# Operator Semantics

## What this document describes

This document records the semantic operator language exposed by the delivered ContextFilter implementation.

It answers:

> What relationship does each operator family express between one parameter state/value and the operands stored in a filter condition?

## Source boundary

The supplied implementation analysis confirms 19 operators grouped as:

| Family | Operators |
|---|---|
| Equality | `Equals`, `NotEquals` |
| String | `Contains`, `NotContains`, `StartsWith`, `EndsWith` |
| Existence / emptiness | `IsEmpty`, `IsNotEmpty`, `Exists`, `NotExists` |
| Numeric | `GreaterThan`, `GreaterThanOrEqual`, `LessThan`, `LessThanOrEqual` |
| Range | `Between`, `NotBetween` |
| List | `InList`, `NotInList` |

This repository preserves those supported distinctions. It does not invent additional coercion, tolerance or locale rules that are not documented in the supplied evidence.

---

## Operator meaning depends on Catalog semantics

Filtering does not receive raw UI strings in isolation. Conditions refer to a canonical `ParameterKey`, and Catalog distinguishes value states such as:

```text
NOT LOADED
MISSING
EMPTY
CONCRETE
```

See [`../catalog/value-semantics.md`](../catalog/value-semantics.md).

This matters because:

```text
NotExists
!= IsEmpty
```

The evaluator must not collapse different source states merely because both might appear visually blank.

---

## Equality family

```text
Equals
NotEquals
```

These operators express equality or inequality against the condition operand according to the current normalized/typed comparison representation.

Important distinction:

```text
Equals("Wall A")
!= Contains("Wall A")
```

The original customer requirement history included a concrete correctness concern where a type-oriented filter could include unrelated types under looser matching behavior. That evidence reinforces that equality semantics are correctness, not just UI wording.

---

## String family

```text
Contains
NotContains
StartsWith
EndsWith
```

These operators express textual relationships rather than exact identity.

The implementation model includes `IgnoreCase` on `FilterCondition`, and the application contains parameter-value normalization logic.

Therefore case handling is part of comparison behavior.

The supplied analysis does not fully define every normalization detail for all string cases, so this document does not add undocumented trimming/culture rules beyond acknowledging that normalization exists in the implementation.

---

## Existence / emptiness family

```text
Exists
NotExists
IsEmpty
IsNotEmpty
```

These operators depend on the value-state distinctions owned by Catalog.

Canonical distinction:

```text
NotExists
→ parameter/property is semantically absent

IsEmpty
→ parameter/property exists but its value is empty
```

This is explicitly reflected in the delivered quick-filter behavior:

```text
__missing__ → NotExists
__empty__   → IsEmpty
```

Therefore these special values are not interchangeable aliases.

Also:

```text
NOT LOADED
!= NotExists
```

A light snapshot that has not yet materialized a parameter cannot safely satisfy `NotExists` merely because the parameter dictionary does not currently contain it.

---

## Numeric comparison family

```text
GreaterThan
GreaterThanOrEqual
LessThan
LessThanOrEqual
```

These operators require a value representation that can participate in numeric comparison.

The delivered Catalog supports typed parameter values and a Revit-to-domain conversion layer.

Therefore numeric semantics belong to typed filterable knowledge, not to lexical comparison of display strings.

```text
numeric comparison
!= compare formatted UI text alphabetically
```

The supplied analysis does not document unit-conversion/tolerance rules in sufficient detail to make them canonical here. Those details must not be fabricated.

---

## Range family

```text
Between
NotBetween
```

These operators express membership inside or outside a range and therefore require multiple operands.

The exact inclusive/exclusive boundary behavior is not explicitly documented in the supplied project analysis.

Accordingly, this repository records the operator family but does not invent boundary rules. If range semantics become important to future maintenance, the delivered implementation/test behavior should be used to make inclusivity explicit.

---

## List family

```text
InList
NotInList
```

These operators compare one source value against a collection of allowed/disallowed operands.

Implementation evidence shows `InList` is also used by the quick-filter authoring path for multiple selected values.

```text
selected values [A, B, C]
→ InList(A, B, C)
```

This is semantically different from an AND group requiring equality to A, B and C simultaneously.

---

## Operator arity

Different operator families need different operand structures.

Conceptually:

```text
Exists / NotExists / IsEmpty / IsNotEmpty
→ state-oriented; no concrete comparison value required

Equals / Contains / numeric comparisons
→ one comparison operand

Between / NotBetween
→ range operands

InList / NotInList
→ operand collection
```

The exact validation mechanics of malformed operand sets are not fully described in the supplied analysis and therefore remain implementation validation behavior rather than invented canonical rules here.

---

## Type compatibility

A filter operator must be meaningful for the current parameter/value representation.

For example:

```text
numeric operator
→ requires numeric-compatible value knowledge
```

while string operations depend on text-compatible representation.

The implementation contains typed `ParameterValueKind` values and a converter/normalizer layer. This is evidence that comparison semantics are not intended to be one universal string comparison.

---

## Operator semantics != native Revit compatibility

The client-side filter language contains operators that the supplied analysis says cannot always be converted into native Revit filters.

Examples listed as unsupported for native conversion include several string and range constructs.

Therefore:

```text
operator supported by ContextFilter semantic evaluator
!= operator supported by native Revit filter conversion
```

The semantic language is canonical for ContextFilter filtering. Native support is a separate capability boundary.

---

## Invariants

1. Operator labels represent different semantic relationships and must not be loosened silently.
2. `NotExists` and `IsEmpty` operate on different Catalog value states.
3. Numeric comparison must rely on numeric-compatible knowledge, not display-string ordering.
4. `InList` is not equivalent to an AND group of equality conditions.
5. Case policy is part of condition semantics where applicable.
6. Native Revit support does not define the limits of ContextFilter's semantic filter language.
7. Undocumented edge rules such as range inclusivity or numeric tolerance must be verified from implementation before being made canonical.

---

## Check

For any operator use we should be able to answer:

- what parameter identity is being evaluated;
- what source value state is present;
- what typed comparison representation is required;
- how many operands are required by the operator family;
- whether case policy affects comparison;
- whether missing and empty states remain distinct;
- whether the operator is supported semantically even if native Revit conversion is unavailable;
- which edge semantics are verified and which remain unspecified by current evidence.
