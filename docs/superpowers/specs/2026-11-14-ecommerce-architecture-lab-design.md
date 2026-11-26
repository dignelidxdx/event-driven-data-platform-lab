# Design: Software architecture lab applied to an e-commerce system

## Intent

Build an executable, educational lab for studying software architecture applied to an e-commerce system, connecting DDD, clean architecture, hexagonal architecture, CQRS, events, security, observability, schema evolution, and progressive migration toward microservices.

The lab will use an evolving modular monolith in TypeScript. Modules will be separated by domain and expose explicit contracts. The application may evolve toward microservices when technical reasons and metrics justify it.

## Modules

| Module | Responsibility |
|---|---|
| `catalog` | Products, pricing, and availability |
| `cart` | Cart and line items |
| `orders` | Orders and lifecycle |
| `payments` | Payment port and demonstration adapter |
| `identity` | Identity, roles, and conceptual authorization |
| `architecture-lab` | Examples, diagrams, metrics, and educational material |

Build order: `catalog` → `cart` → `orders` → `identity` and `payments` → `architecture-lab`.

## Logical architecture

Each module will follow concentric layers:

```text
external adapters → application → domain
```

- Domain: entities, value objects, invariants, and domain events.
- Application: use cases, coordination, transactions, and ports.
- Adapters: HTTP, persistence, clock, payments, and event publishing.
- Infrastructure: dependency composition, configuration, and observability.

## Demonstration flow

1. Browse the catalog.
2. Add a product to a cart.
3. Confirm the cart.
4. Create an order with a demonstration identity.
5. Publish an `OrderPlaced` event.
6. Update a query projection and request payment through a port.

## Educational content

The documentation will cover fundamentals, DDD, bounded contexts, context mapping, clean architecture, hexagonal architecture, microkernel, microservices, events, CQRS, event sourcing, API Gateway, Conway's law, SOLID, cohesion, coupling, interfaces, abstractions, DRY, antipatterns, Flyway, ETL/ELT, OLTP/OLAP, data mesh/fabric, security, metrics, SLI/SLO/SLA, p95, error budgets, pre-mortems, post-mortems, five whys, the strangler pattern, and the Big Ball of Mud.

It will include a Cartesian plane with instability on the X axis and abstraction on the Y axis:

- Zone of pain: high concreteness and high incoming dependency.
- Zone of uselessness: high abstraction and low real dependency.

## Initial metrics

The catalog will have the following proposed metrics:

- Availability: successful requests divided by total requests.
- Latency: proportion of requests below the threshold.
- Error budget: `1 - 0.995 = 0.005`, approximately 3 h 39 min over 30 days.

Instrumentation will start with the first relevant endpoint, and targets will be tightened when a representative baseline exists.

Persistence will sit behind ports. The lab will explain Flyway, compatible migrations, expand-and-contract, backfills, and lock management. Security will distinguish identity, authentication, authorization, sessions, hashing, and encryption, while avoiding custom cryptography. The pipeline will include reproducible installation, formatting, linting, tests, dependency analysis, building, controlled migration, and a smoke test.

## Success criteria

- The application starts with reproducible instructions.
- The catalog → cart → order flow works end to end.
- Modules do not depend directly on concrete adapters.
- The README links to documentation, ADRs, and SVG diagrams.
- SLI, SLO, and SLA are differentiated with formulas.
- Progressive extraction through the strangler pattern is documented.

## Pending technical decisions

The HTTP framework, database, web interface, and event adapter will be selected according to the minimum infrastructure required to run and test the lab.
