# Parameter Identity Model

## What this document describes

This document defines **how ContextFilter identifies a filterable parameter independently from the label shown to the user**.

It answers:

> When the user selects “Марка”, “Комментарии”, “Тип” or another visible parameter name, what canonical identity does the system actually carry into filtering and later Revit realization?

## What it intentionally does not describe

It does not define:

- how parameter values are compared — [`value-semantics.md`](value-semantics.md) and [`../filtering/`](../filtering/);
- how a semantic `ParameterKey` is resolved back to native Revit API objects — [`../revit-boundary/`](../revit-boundary/);
- UI grouping/order of parameter labels.

## Implementation evidence

The delivered domain model contains:

```text
ParameterKey =
    IdentityKind
  + IdentityValue
  + Source
```

Current evidence names identity kinds including:

- built-in parameter;
- shared parameter;
- project parameter;
- synthetic.

Current evidence names sources:

- instance;
- type;
- synthetic.

The snapshot separately stores:

```text
ParameterKey → ParameterValue
ParameterKey → DisplayName
```

This is the central evidence for the distinction between identity and presentation.

## Canonical identity

The canonical identity used by ContextFilter is not:

```text
"parameter visible name"
```

It is the semantic key carried through the system:

```text
IDENTITY KIND
      +
IDENTITY VALUE
      +
SOURCE
      ↓
PARAMETER KEY
```

The exact physical value stored in `IdentityValue` depends on the identity kind and is implementation-specific. This repository does not invent stronger rules than the implementation evidence establishes.

## Why display name is insufficient

Two parameters may be presented with equal or similar text while differing by source or native identity.

Even without relying on a particular collision example, the implementation itself proves that the system does not trust display text as identity because it maintains a separate `ParameterKey`.

Therefore:

```text
display name
→ user-facing description

ParameterKey
→ semantic identity used by system behavior
```

Canonical rule:

> **A display label may be changed, localized or duplicated without automatically changing which parameter the filter means.**

## Parameter source

Source is part of the identity model because instance and type parameters are not interchangeable facts.

```text
INSTANCE SOURCE
value belongs to the element instance representation

TYPE SOURCE
value is obtained through the element's type representation

SYNTHETIC SOURCE
value is produced by ContextFilter from other source facts
```

Therefore:

```text
same visible label + different source
!= automatically same filterable parameter
```

## Identity kind

Identity kind records **what kind of identity evidence makes this parameter resolvable**.

Current implementation evidence distinguishes at least:

```text
BuiltInParameter
SharedParameter
ProjectParameter
Synthetic
```

This prevents a single string namespace from pretending all Revit parameter mechanisms have identical identity semantics.

## Synthetic identities

Synthetic parameters are first-class filterable identities even though they are not ordinary native Revit parameters.

Current evidence includes:

```text
Category
Family
TypeName
ElementId
UniqueId
Workset
Level
```

Their flow is:

```text
source Revit facts
→ synthetic provider / snapshot construction
→ Synthetic ParameterKey
→ ParameterValue
→ same semantic filter engine
```

This yields an important distinction:

> **Same filter interface does not imply same source mechanism.**

A condition over `Category` and a condition over a built-in Revit parameter can be expressed through the same filter model while requiring different native realization rules.

## Identity through the system

```text
CATALOG
exposes ParameterKey + label
        ↓
USER FILTER INTENT
selects ParameterKey
        ↓
FILTER DEFINITION
stores ParameterKey
        ↓
FILTER EVALUATION
evaluates snapshot value under that key
        ↓
OPTIONAL NATIVE REALIZATION
Revit boundary attempts to resolve compatible keys
```

The system should not downgrade the identity back to display text between these stages.

## Semantic filter vs native resolvability

A valid `ParameterKey` in the semantic filter model does not guarantee that it can be represented by a native Revit filter.

For example, synthetic identities are explicitly produced by the plugin rather than ordinary native Revit parameters.

Therefore:

```text
valid filterable identity
!= guaranteed native Revit parameter identity
```

Native compatibility belongs to [`../revit-boundary/`](../revit-boundary/).

## Persistence implication

Presets persist filter intent containing parameter identities.

That means a persisted preset should conceptually remember **which parameter was meant**, not merely which label was visible when it was saved.

Exact schema/migration rules remain owned by [`../presets/`](../presets/), but Catalog provides the identity semantics on which those persisted references depend.

## Change impact

This model must be reopened when any of the following change:

- a new parameter identity kind is introduced;
- source semantics change;
- synthetic parameters are added/removed/redefined;
- native Revit identity mapping changes in a way that affects semantic uniqueness;
- persisted parameter references require a new canonical representation.

A label text change alone does not automatically require reopening canonical identity.

## Invariants

1. Display name is presentation, not canonical parameter identity.
2. Identity kind, identity value and source jointly define the semantic key used by ContextFilter.
3. Instance and type sources remain distinct unless an explicit rule says otherwise.
4. Synthetic properties are first-class filterable identities but not ordinary native Revit parameters.
5. Filter definitions carry identity, not only display labels.
6. Native Revit resolvability is a separate question from semantic filterability.

## Typical analytical mistakes

### Keying filters by localized name

This collapses presentation and identity and makes saved intent fragile.

### Treating type and instance values as one namespace

This loses where the fact came from and can change filter meaning.

### Treating synthetic values as fake data

They are derived, but still canonical within ContextFilter's filterable representation when their derivation rule is explicit.

### Assuming every ParameterKey can become a ParameterFilterElement rule

Semantic identity and native-filter compatibility are separate responsibilities.

## Check

For any filterable parameter we should be able to answer:

- what identity kind it uses;
- what identity value is carried;
- whether it is instance, type or synthetic;
- what label is shown to the user;
- whether changing that label changes identity;
- whether the Revit boundary can resolve it natively;
- whether a persisted preset can preserve the same intent later.
