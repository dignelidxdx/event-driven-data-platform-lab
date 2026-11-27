# B2 — Design without implementation: Clinical order platform

This document preserves an alternative domain for future learning. It authorizes no code, real data, clinical integrations, or medical decisions.

## Intent

Design a fictional platform that coordinates a clinical order across prescription, validation, laboratory, and notification. It is intended to study DDD, context boundaries, eventual consistency, and the Saga pattern when compensations are genuine.

An order crosses several autonomous systems. It may be rejected during validation, run out of laboratory capacity, or require cancellation. A single database transaction cannot govern these distributed systems.

## Contexts

| Context | Responsibility | Events |
|---|---|---|
| Order Management | Create, query, and cancel orders | `ClinicalOrderCreated`, `ClinicalOrderCancelled` |

- Each context is a module with its own model and versioned event contracts.
- Each step has explicit compensation when possible: if a reservation fails after approval, the order is cancelled or the corresponding resource is released.
- A query view may materialize order state; CQRS would be introduced only if reads and writes actually diverge.
- Event Sourcing would not be used initially: an event-based audit history alone does not justify rebuilding the aggregate from events.

## Educational risks and safeguards

| Risk | Safeguard |
|---|---|
| Confusing the domain with real medical software | Fully synthetic data and non-operational clinical language |
| Coupling between contexts | Minimal public events and versioned contracts |

## What this would teach

- When a Saga is necessary and when it is not.
- The difference between choreography and orchestration.
- Compensation versus distributed rollback.
- Observability for long-running transactions.

The B1 flow is better preparation for the immediate interview: it aligns directly with a Data Platform, ingestion, messaging, processing, and cloud operations. B2 is retained as the next learning module, separated to avoid contaminating the initial scope.

## Activation criteria

- The B1 lab is implemented, tested, and explainable.
- There is specific interest in practicing Saga and eventual consistency across contexts.
- A clear synthetic-data policy and strictly educational scope are defined.
