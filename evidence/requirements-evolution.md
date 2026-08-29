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

## Stage 4 — user testing and stabilization

User testing exposed several behaviors that were not adequately covered by the initial requirement drafts. They were treated as new system evidence and resulted in changes to the delivered behavior.

### Persisted settings must be validated

Incorrect user configuration could cause runtime failures.

```text
persisted settings
→ deserialize
→ validate + normalize
→ safe runtime settings
```

The system therefore treats stored configuration as input that must be checked rather than automatically trusted.

### Preset-load failure must be visible

Failure to load presets previously lacked clear user feedback. The delivered behavior now reports that failure instead of silently presenting an apparently valid state.

### Revit document lifecycle bounds context validity

Background work previously continued after a document was closed, and UI state could survive a document switch.

The corrected behavior is:

```text
document closes / changes
→ invalidate document-bound context
→ stop obsolete background work
→ reset document-specific UI state
```

### Background activity is session-bound

The plugin initially opened automatically with Revit and event handlers could remain active even when the filter was not being used. This produced unnecessary load.

The stabilized behavior moved to explicit launch and session-scoped background processing:

```text
panel closed / no active filter session
→ no heavy collect / index / highlight work
```

Built-in presets are also initialized on first panel use rather than during Revit startup.

### Convenience must not break host interaction

Hotkeys were originally active by default. They became opt-in after testing.

A `Ctrl + Click` interaction conflicted with native Revit multi-selection and was replaced with `Ctrl + Shift + Click`.

This yields a broader rule:

> **Plugin interaction must not silently steal established host-application semantics.**

### Revit actions must obey host write rules

Isolation behavior initially attempted a Revit operation outside the required transaction context. The implementation was corrected so host-side actions cross the Revit boundary using valid execution/transaction mechanics.

### Shutdown behavior is part of runtime correctness

Delays during Revit shutdown led to changes in plugin teardown and resource-release behavior. A successful filtering session is therefore not the only runtime concern; startup, idle state, document changes and shutdown are part of the system lifecycle.

## Confirmed project context

The project delivery context was confirmed separately from the source artifacts:

- the author performed **requirement collection, system analysis, solution design, development and deployment**;
- the customer role was held by a **deputy director of a design institute**;
- a **BIM coordinator** acted as the domain expert;
- the organization name is intentionally not published;
- final acceptance was performed by the **director of the institute**;
- the plugin was deployed and is in use;
- no errors or complaints were reported after deployment;
- the exact user population is not claimed because it was not tracked for this public case.

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

first implementation
!= final system knowledge
```
