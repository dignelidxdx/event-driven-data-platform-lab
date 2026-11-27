# Distributed Systems

Distributed systems coordinate work across processes or machines. Network delay, partial failure, concurrent updates, and independent deployment affect correctness and operations. Introduce these costs when they solve a real requirement.

## Event-driven architecture

Producers publish facts or requests that consumers process asynchronously. This can reduce temporal coupling and support multiple consumers, while making delivery, ordering, replay, observability, and eventual consistency explicit concerns. Name domain events as past-tense facts.

## CQRS

Separates command handling that changes state from query models optimized for reads. CQRS can help when read and write needs differ materially; separate models and synchronization are unnecessary complexity for many applications.

## Event sourcing

Stores state changes as an append-only event history and derives current state by replay or projection. It supports audit and temporal reconstruction when those are core needs, while increasing schema evolution, replay, projection, and operational complexity. It is not a prerequisite for event-driven architecture.

## Saga

Coordinates a business transaction spanning multiple local transactions using steps and compensating actions. A saga handles distributed workflows without a single global transaction, but compensation is business behavior and may not perfectly undo an action. Choose choreography or orchestration according to workflow visibility and coordination needs.

## Consistency and CAP

Consistency models describe what reads can observe relative to writes. CAP is a constrained result about behavior during a network partition, not a general rule that systems choose any two properties at all times. State the failure scenario and guarantees before applying it to a design.

See [data and storage](data-and-storage.md) and the Delivery Framework's [data-flow](../delivery-framework/04-data-flow.md) and [deep dives](../delivery-framework/06-deep-dives.md).
