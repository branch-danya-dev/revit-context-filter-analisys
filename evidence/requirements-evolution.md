# Requirements Evolution

This document reconstructs **how the ContextFilter problem and requested behavior evolved** without turning historical requirement documents into the current repository structure.

## Stage 1 — original operational problem

The initial requirement described a repetitive Revit workflow:

```text
find a representative element on a 3D / plan / section view
→ select it manually
→ use native “Select All Instances” style behavior
→ then hide / isolate / work with similar elements
```

The desired improvement was a fast contextual way to work with:

- categories;
- families;
- family types;
- parameter-based filtering;
- selection;
- hiding;
- isolation;
- active-view or wider model context.

A concrete correctness concern was also recorded: filtering by a type value such as a wall type should not include unrelated types merely because a looser condition happens to match.

This became an important analytical signal:

> **Filter operator semantics are part of correctness, not merely UI convenience.**

## Stage 2 — refined interaction and scope

A later requirement draft made the workflow much more explicit.

### Context

The user could begin with a manually selected subset or leave selection empty and work over the current view.

### Interaction

The filter UI was described as three sequential zones:

```text
category
→ parameter
→ value / condition
```

with AND / OR semantics for combining criteria.

### Requested behaviors

The refined draft added or clarified:

- dynamic highlighting;
- hide / isolate;
- exclusion from an existing selection;
- inverse isolation;
- reusable presets;
- templates without fixed values;
- grouping around common parameter values;
- multiple launch modes / hotkeys;
- Revit 2025 as the supported target.

The same source explicitly prohibited deletion, movement or arbitrary modification of matched elements.

## Stage 3 — implemented system

Implementation evidence shows a broader but coherent realization of the same problem.

### Explicit scope model

```text
ActiveView
EntireDocument
CurrentSelection
```

### Stable derived model

The implementation separates:

```text
Revit source elements
→ context / snapshots / catalog
→ semantic filter definition
→ matched set
→ action
```

### Richer filter semantics

The delivered filter model supports nested logical groups and operators covering equality, strings, existence/emptiness, numeric comparisons, ranges and lists.

### Additional realized capabilities

Implementation evidence also includes:

- selection modes Replace / Add / Exclude;
- temporary visibility Hide / Isolate / Inverse / Reset;
- native `ParameterFilterElement` creation where compatible;
- filter history;
- Full vs Template presets;
- cache invalidation and incremental patching;
- chunked collection for larger contexts;
- request coalescing;
- explicit ExternalEvent boundary between WPF and Revit API.

These implementation capabilities are not retroactively presented as if every detail had existed in the first customer draft. They are current system evidence.

## Requirement evidence != current authority

Historical documents answer:

> What problem was requested and how was it refined?

The current SSAD model answers:

> What system now exists, who owns each responsibility, and which claims must be reopened when the system changes?

Therefore:

```text
historical wording
!= canonical repository architecture

requested UI representation
!= permanent semantic ownership

implementation class
!= responsibility owner
```

## Open presentation context

The supplied artifacts establish customer-originated requirements and implemented behavior, but they do not by themselves establish every public-facing project fact such as personal role, team composition, acceptance process or deployment scale. Those facts should be added only from separately confirmed project evidence.
