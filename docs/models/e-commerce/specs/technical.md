# E-commerce Architecture Lab Technical Specification

## Status and traceability

This document captures the current architecture direction for the learning application. The [functional specification](functional.md) states the intended learner and demonstration behavior. Choices marked open must be resolved before implementation.

## Domain model and boundaries

Candidate modules and responsibilities:

| Module | Responsibility | Candidate core concepts |
|---|---|---|
| Catalog | Products, pricing, availability | Product, Price, Availability |
| Cart | Selected items before confirmation | Cart, CartLine |
| Orders | Accepted purchase and lifecycle | Order, OrderLine, OrderStatus |
| Payments | Payment boundary and demonstration adapter | PaymentRequest, PaymentResult |
| Identity | Demonstration identity and conceptual authorization | User/Principal, Role |
| Architecture lab | Examples, diagrams, metrics, and learning material | Educational artifact |

These are provisional boundaries, not proof that each module should become a service. Define ownership, invariants, and permitted imports as modules are implemented. Candidate invariants include positive quantities, valid product availability at cart confirmation, and an explicit order lifecycle. Price snapshot semantics and payment failure behavior remain open business decisions.

## Workload and scale

No end-user traffic, DAU/MAU, request rate, data retention, or production SLO has been specified. Since this is initially a local learning application, do not invent scale targets. If it becomes a deployed product, measure or obtain:

- active users and session/request patterns (DAU/MAU only if user activity is the relevant product measure);
- catalog reads, cart writes, checkout attempts, and payment requests per second, including peak bursts;
- catalog size, cart/order growth, and retention;
- checkout latency and availability expectations;
- payment-provider limits and failure behavior.

Use these measurements to evaluate caching, database choice, concurrency, and scaling. The current design does not justify a load balancer, CDN, broker, or service split by default.

## Quality attributes and metrics

Candidate measures for an implemented demonstration include catalog request success/latency, cart-to-order conversion in synthetic flow tests, order creation success, payment-adapter outcomes, and event projection lag. These are measurement candidates, not targets. Specify denominators, windows, and thresholds before defining SLI/SLO commitments.

## Logical architecture

The proposed module shape is a modular monolith with concentric responsibilities:

```text
external adapters → application → domain
```

- Domain: entities, value objects, invariants, and domain events.
- Application: use cases, coordination, transactions, and ports.
- Adapters: HTTP/UI, persistence, clock, demonstration payment, and event publishing.
- Infrastructure: composition, configuration, and observability.

Dependencies should point inward toward domain/application policy. Each module should state which other module interfaces it may import. Consider service extraction only when measured technical constraints, ownership, or deployment needs justify its cost.

## Main demonstration flow

1. Browse catalog.
2. Add product to cart.
3. Confirm cart.
4. Create order with demonstration identity.
5. Publish `OrderPlaced`.
6. Update query projection and request payment through a port.

The exact UI and transport are open. Keep contracts explicit whether the first adapter is CLI, HTTP, or web UI.

## Data and integration

Persistence and payment integrations sit behind ports. Keep events in past tense and include only required fields. CQRS is an educational option if read/write models materially differ; event sourcing, brokers, and microservices are not initial requirements. Explain transaction boundaries, projection consistency, retries, and schema evolution when implementation reaches those concerns.

## Technology selection

The existing project direction uses strict TypeScript and English public identifiers. HTTP framework, persistence engine, web interface, and event adapter remain undecided. Select them based on the smallest executable local stack, team learning goals, testability, support needs, and operational cost. Record consequential choices in ADRs rather than preselecting tools in this spec.

## Security and privacy

Use synthetic commerce data and demonstration identity/payment only. Distinguish authentication from authorization; validate inputs at boundaries; do not implement custom cryptography or handle real payment credentials. Add real identity, payment providers, or production secrets only through a separately reviewed scope.

## Validation and delivery

- Test domain invariants and use cases independently of adapters.
- Add integration/contract tests when persistence, HTTP, or event boundaries are selected.
- Demonstrate the catalog-to-order flow end to end.
- Measure relevant metrics only after an implementation baseline exists.
- Document migration, rollback, observability, and deployment only when the corresponding infrastructure is selected.

## Open decisions

- User interface and primary entry point.
- HTTP framework and endpoint contracts.
- Persistence technology and migration mechanism.
- Price snapshot, cart expiry, order cancellation, and payment-failure rules.
- Whether a query projection is needed for the first executable slice.
