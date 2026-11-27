# Repository Strategy

Choose repository boundaries according to how code changes, builds, ownership, and releases fit together. Monorepo and multirepo are trade-offs, not maturity levels.

| Consideration | Monorepo | Multirepo |
|---|---|---|
| Cross-component changes | One review can update coordinated modules and contracts | Changes require coordinated PRs or published contracts |
| Versioning and releases | Can share tooling while releasing components together or independently | Repositories naturally isolate version and release workflows |
| Ownership | Requires clear module ownership and dependency boundaries | Repository ownership can align with team autonomy, but shared standards need coordination |
| Tooling and scale | May need build graph, caching, and selective CI as it grows | Simpler per-repo tooling, with repeated configuration and dependency updates |
| Access and visibility | Broad repository access is common; finer access can be harder | Separate access controls and visibility are easier to express |

## Decide with evidence

Ask whether components change together, need independent release and access controls, share build tooling, or have distinct owners. Consider repository size, CI duration, dependency management, code search, compliance, and developer onboarding. A monorepo still needs modular architecture; separate repositories do not automatically create independent services.

## Example

A modular monolith with catalog, cart, and orders modules may benefit from one repository because a domain change often updates several contracts atomically. Independently owned services with separate release cadences and access requirements may justify multiple repositories. Validate the choice against actual team and delivery constraints, then record a consequential decision in an ADR.
