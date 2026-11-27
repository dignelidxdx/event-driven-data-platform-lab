# Architecture Patterns

Architecture patterns describe recurring ways to divide responsibilities and dependencies across a system. A pattern is a candidate structure, not a guarantee of quality; select it according to requirements, team context, and operating costs.

## Layered architecture

Organizes code into layers with defined responsibilities, often presentation, application, domain, and infrastructure. It is easy to explain and can fit cohesive applications. Uncontrolled cross-layer access or a database-centered domain can weaken the boundaries.

## Ports and adapters (hexagonal architecture)

Places application and domain logic behind ports, with adapters translating external technologies into those interfaces. It supports isolated domain testing and replaceable integrations. Too many abstractions for stable or trivial boundaries add indirection without useful flexibility.

## Clean architecture

Groups code into concentric policy boundaries where source dependencies point toward inner business rules. It overlaps with ports and adapters in its dependency direction. The names are less important than enforceable boundaries and explicit dependency rules.

## Modular monolith

Deploys one application while maintaining explicit internal modules and contracts. It can keep operations simpler than distributed services while allowing independent domain organization. Shared databases and weak module boundaries can create coupling.

## Microservices

Splits a system into independently deployable services, often organized around business capabilities. It can support independent ownership and scaling, while adding network failure, data consistency, deployment, and observability costs. Extract services when there is a concrete organizational or technical reason.

## Microkernel

Keeps a small core and extends it through plugins or replaceable modules. It suits systems whose capabilities need controlled extension; plugin lifecycle, compatibility, and isolation need deliberate design.

Compare patterns in the [System Design Process options stage](../system-design-process/05-options-and-trade-offs.md) and express the chosen structure in [high-level design](../delivery-framework/05-high-level-design.md).
