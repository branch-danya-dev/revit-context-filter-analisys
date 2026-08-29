# Presets and History

This area owns **reusable user filtering intent that persists beyond one transient filter evaluation**.

## Canonical question

> Which parts of a filter session may be saved, restored or reused later without pretending that saved values are current Revit truth?

## Preset kinds

The implemented model distinguishes:

```text
Full preset
→ stores a reusable filter including concrete values

Template preset
→ stores filter structure without binding every value
```

Templates are important when the same analytical intent must be reused across projects whose actual parameter values differ.

## History

Recent filters may be stored for convenient replay. History is convenience memory, not an audit log of model truth.

## Ownership

ContextFilter owns:

- preset identity and schema;
- saved filter structure;
- preset kind;
- local history ordering/deduplication;
- schema migration of its own persistence format.

Revit still owns the parameter values and element state against which a restored preset is evaluated.

## Validity on restore

```text
Saved intent
→ restore into current session
→ resolve against current catalog/context
→ evaluate now
```

A preset must not assume that categories, parameters or values still exist simply because they existed when saved.

## Physical persistence

The current implementation uses local JSON files for presets/history/settings. JSON is the storage representation; it is not the semantic owner.

## Does not own

- current Revit context → [`../context/`](../context/);
- filter semantics → [`../filtering/`](../filtering/);
- filesystem mechanics and runtime recovery details → [`../runtime/`](../runtime/).
