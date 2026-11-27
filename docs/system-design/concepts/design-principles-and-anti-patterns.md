# Design Principles and Anti-Patterns

Use principles to evaluate whether a design remains understandable and changeable. They are heuristics whose value depends on context, not a checklist that justifies adding abstractions.

## SOLID

- **Single Responsibility:** group behavior around a cohesive reason to change.
- **Open/Closed:** make likely variation possible without editing stable policy everywhere.
- **Liskov Substitution:** implementations should honor the behavior callers rely on.
- **Interface Segregation:** keep interfaces focused on what their clients need.
- **Dependency Inversion:** make high-level policy independent of low-level details through deliberate boundaries.

## Cohesion and coupling

Keep related behavior together and minimize unnecessary dependency knowledge between modules. Explicit contracts and ownership help modules evolve independently.

## Common anti-patterns

- **Big Ball of Mud:** boundaries and responsibilities have eroded until changes create unpredictable effects.
- **Spaghetti code:** control flow and dependencies are tangled, making behavior hard to trace.
- **Premature abstraction:** generality is added before repeated needs or useful variation are understood.
- **Distributed monolith:** components are deployed separately but remain tightly coordinated and fail together operationally.

Look for evidence: repeated changes across unrelated modules, cyclic dependencies, unclear ownership, or slow and risky changes. Refactor toward a clear boundary that addresses an observed problem; do not split code solely to match a pattern name.
