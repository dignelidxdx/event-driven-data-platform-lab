# Architecture Decision Record Template

## Title

Use a short statement naming the decision.

## Status

Proposed, accepted, superseded, or deprecated.

## Context

Describe the forces, requirements, and constraints that make a decision necessary.

## Decision

State the chosen approach clearly.

## Alternatives considered

List credible alternatives and why they were not selected.

## Consequences

Record benefits, costs, risks, and follow-up work, including consequences that become visible only operationally.

## Verification and architectural fitness functions (optional)

For decisions with important properties that could erode as the system changes, record how the team will check them. A fitness function is the repeatable check; it may be a test, static analysis rule, contract check, or operational measurement. Not every ADR needs one.

- **Property or constraint to preserve:** What must remain true?
- **Check:** What will verify it (for example, a dependency rule, contract test, integration test, or measured quality check)?
- **When it runs:** For example, locally, in continuous integration (CI) on each pull request, or in production monitoring.
- **Passing and failing criteria:** What result is acceptable, and what should happen when the check fails?
- **Limitations or assumptions:** What does the check not prove, and what conditions does it rely on?

Prefer the smallest reliable check for the property. Functional and integration tests can serve as fitness functions when they enforce an architectural rule or quality; their test level alone does not make them fitness functions. CI can run automated checks and block a change on failure. Continuous delivery/deployment (CD) concerns release and deployment, and may use related checks as gates, but is not required for a fitness function.

## References

Link requirements, RFC, diagrams, experiments, and affected contracts.
