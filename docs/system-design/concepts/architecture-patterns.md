# Architecture Patterns

Architecture patterns describe recurring ways to divide responsibilities and dependencies across a system. A pattern is a candidate structure, not a guarantee of quality; select it according to requirements, team context, and operating costs.

## Domain-Driven Design (DDD)

DDD is an approach to modeling software around the business domain, especially when its rules and language are complex. It uses a shared domain vocabulary, bounded contexts, and tactical patterns such as entities, value objects, and aggregates to make rules explicit. For example, an e-commerce system may distinguish the Catalog's `Product` from the Order context's `OrderLine`, even when both refer to the same item.

**Consider DDD when** business rules are complex, the same terms mean different things across teams, or changes repeatedly expose unclear ownership and boundaries. Validate the model with domain experts. DDD can be used inside a modular monolith; it does not require microservices. Its modeling terms and ceremonies add little value when the domain is simple or the team cannot validate the model with business stakeholders. See [Core Entities](../delivery-framework/02-core-entities.md) for where domain discovery fits in the delivery flow.

## Layered architecture

Organizes code into layers with defined responsibilities, often presentation, application, domain, and infrastructure. It is easy to explain and can fit cohesive applications. Uncontrolled cross-layer access or a database-centered domain can weaken the boundaries.

**Consider layered architecture when** the application has clear technical responsibilities and a straightforward dependency flow, and the team benefits from a familiar structure. Define which layers may call which others; if features frequently cross layers or domain rules become tangled with persistence, strengthen module boundaries or evaluate ports and adapters.

## Ports and adapters (hexagonal architecture)

Places application and domain logic behind ports, with adapters translating external technologies into those interfaces. It supports isolated domain testing and replaceable integrations. Too many abstractions for stable or trivial boundaries add indirection without useful flexibility.

**Consider ports and adapters when** important business behavior must be tested without infrastructure, or external systems and transports are likely to change. Identify real boundaries first (for example, payment provider, database, or message broker); do not create a port for every internal function by default.

## Clean architecture

Groups code into concentric policy boundaries where source dependencies point toward inner business rules. It overlaps with ports and adapters in its dependency direction. The names are less important than enforceable boundaries and explicit dependency rules.

**Consider Clean Architecture when** multiple delivery mechanisms or infrastructure choices must remain outside stable application policies, and dependency direction can be enforced through modules or tooling. Avoid adding layers solely to match a diagram when the application has little domain behavior.

## Onion architecture

Organizes the domain and application core at the center, with infrastructure and external concerns in outer rings. Dependencies point inward, so the core does not depend on a database, web framework, or message broker. It shares this dependency rule with Clean Architecture and hexagonal architecture, but presents it as concentric layers; implementations vary, so document actual allowed dependencies rather than relying on labels.

**Consider Onion Architecture when** domain rules should remain central and independent of persistence, frameworks, and delivery mechanisms, and concentric boundaries help the team reason about dependencies. For a small CRUD application, the extra interfaces and mapping can outweigh the benefit.

## Modular monolith

Deploys one application while maintaining explicit internal modules and contracts. It can keep operations simpler than distributed services while allowing independent domain organization. Shared databases and weak module boundaries can create coupling.

**Consider a modular monolith when** one deployment is operationally desirable but the code has distinct business capabilities, team ownership, or likely future change boundaries. Enforce module APIs and ownership of data; if all modules freely share tables and internals, the structure is only nominal. It is often a good starting point before any service extraction.

## Microservices

Splits a system into independently deployable services, often organized around business capabilities or DDD bounded contexts. It can support independent ownership, release cadence, and scaling when those needs differ across capabilities. Each service also adds network failure, data consistency, deployment, security, and observability costs. For example, separate order and catalog services only when their ownership or scaling needs justify independent operation; otherwise, explicit modules in a monolith may be simpler. Extract services when a concrete organizational or technical driver outweighs the distributed-systems costs.

**Consider microservices when** a capability needs independent deployment, scaling, fault isolation, or ownership and the organization can operate distributed systems. Before extracting one, define its data ownership, API/event contract, on-call ownership, deployment pipeline, and failure behavior. A high request count alone does not prove that a service boundary is needed.

## Strangler Fig pattern

Replaces a legacy system incrementally by routing selected capabilities to a new implementation while the old system continues serving the rest. A routing layer (or facade) directs each request to the legacy or new path; capabilities move over in small slices until the old system can be retired.

**Business example:** an e-commerce team might move product search from a legacy storefront to a new catalog service first, then migrate checkout only after payment, inventory, and order behavior have compatible contracts. During migration, both paths need clear ownership for data writes and a way to reconcile results; avoid letting the legacy and new systems independently update the same records.

Use this when the system must remain available during modernization, the target design can be introduced behind stable interfaces, and the team can migrate and validate bounded capabilities one at a time. It is a poor fit when no reliable routing boundary exists, both systems cannot safely coexist, or the migration period's duplicated operations and data synchronization cost more than a planned replacement. Define cutover, observability, rollback, data ownership, and retirement criteria for each slice.

## Microkernel

Keeps a small core and extends it through plugins or replaceable modules. It suits systems whose capabilities need controlled extension; plugin lifecycle, compatibility, and isolation need deliberate design.

**Consider a microkernel when** customers, tenants, or internal teams need to add optional capabilities through a stable extension point (for example, payment or reporting plugins). Specify plugin APIs, version compatibility, permissions, isolation, and failure handling. If extensions are few and controlled by one team, ordinary modules or Strategy may be simpler.

Compare patterns in the [System Design Process options stage](../system-design-process/05-options-and-trade-offs.md) and express the chosen structure in [high-level design](../delivery-framework/05-high-level-design.md).
