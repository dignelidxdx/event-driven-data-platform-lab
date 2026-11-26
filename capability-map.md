# Capability Map: e-commerce architecture lab

This map indexes the laboratory capabilities. Identifiers are stable and are used to name specifications, plans, and tasks.

| Module id | Responsibility | Depends on |
|---|---|---|
| `catalog` | Products, pricing, and availability | — |
| `cart` | Shopping cart and line items | `catalog` |
| `orders` | Orders, invariants, and lifecycle | `cart`, `identity` |
| `identity` | Identity, roles, and conceptual authorization | — |
| `payments` | Payment port and demonstration adapter | `orders` |
| `architecture-lab` | Diagrams, metrics, ADRs, and examples | all modules |

Build order: `catalog` → `cart` → `identity` → `orders` → `payments` → `architecture-lab`.

## Gates

Each module follows `Specify → Plan → Tasks → Implement → Verify`. A module must not be implemented until its specification is approved and its dependencies have clear contracts.

## Boundary criteria

- Dependencies point in one direction.
- Interfaces live at the boundary that needs them.
- Modules do not share internal entities.
- Integration events are explicit contracts.
- Infrastructure does not leak into the domain.
