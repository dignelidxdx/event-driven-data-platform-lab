# Distributed Systems

Distributed systems coordinate work across processes or machines. Network delay, partial failure, concurrent updates, and independent deployment affect correctness and operations. Introduce these costs when they solve a real requirement.

## Event-driven architecture

Producers publish facts or requests that consumers process asynchronously. This can reduce temporal coupling and support multiple consumers, while making delivery, ordering, replay, observability, and eventual consistency explicit concerns. Name domain events as past-tense facts.

**Consider event-driven architecture when** producers and consumers need independent lifecycles, one business fact has multiple downstream uses, or work can complete asynchronously. Define delivery guarantees, event ownership and schema evolution, ordering needs, duplicate handling, replay policy, and acceptable propagation delay. A direct synchronous call is usually simpler when one caller needs an immediate result from one owner.

## CQRS

Separates command handling that changes state from query models optimized for reads. CQRS can help when read and write needs differ materially; separate models and synchronization are unnecessary complexity for many applications.

**Consider CQRS when** read traffic or query shapes differ substantially from write rules, read models need independent optimization, or commands must enforce explicit domain behavior. Start with separate code paths in one application if that solves the problem; separate databases and asynchronous projections add lag, reconciliation, and operational work and should be justified by requirements.

## Event sourcing

Stores state changes as an append-only event history and derives current state by replay or projection. It supports audit and temporal reconstruction when those are core needs, while increasing schema evolution, replay, projection, and operational complexity. It is not a prerequisite for event-driven architecture.

**Consider Event Sourcing when** the authoritative history of business decisions is itself required (for example, regulated audit or reconstructing how an order reached its current state), and the team can version events and operate projections and replays. It is not simply a way to publish events. If current state plus ordinary audit records meets the requirement, conventional persistence is usually simpler.

## Saga

Coordinates a business transaction spanning multiple local transactions using steps and compensating actions. A saga handles distributed workflows without a single global transaction, but compensation is business behavior and may not perfectly undo an action. Choose choreography or orchestration according to workflow visibility and coordination needs.

**Consider a Saga when** one business workflow spans independently committed services or resources and a single ACID transaction is unavailable or undesirable. Identify each step, timeout, retry, compensation, terminal failure, and manual-recovery path. Compensation must be valid business behavior (a refund is not the same as erasing a charge); for a single database transaction, use a local transaction instead.

**Example:** checkout may reserve inventory, authorize payment, and arrange shipment. If shipment cannot be arranged after payment authorization, the workflow may release the inventory reservation and void or refund the authorization, according to the payment provider's rules. Record each step and outcome so retries and manual recovery can continue safely.

## Idempotency

An operation is idempotent when repeating the same request or message produces the same intended business outcome as processing it once. This matters because clients, brokers, and workers retry after timeouts or uncertain acknowledgements: the first attempt may have succeeded even though its response was lost. Idempotency prevents a retry from creating a second order, charging a customer twice, or applying the same event twice.

**Consider explicit idempotency controls whenever** a caller or transport may retry a state-changing operation, delivery is at least once, or a timeout can leave the result uncertain. Decide the operation identity, key scope, retention period, atomic recording mechanism, behavior for key reuse with a different payload, and what duplicate callers receive. For naturally idempotent state-setting operations, document and test that property; do not add a key store without a retry or duplicate-effect risk.

**Payment API example:** the client sends a stable idempotency key with a payment request. The server atomically records the key, request identity, and outcome with the payment operation. A retry with the same key and same request returns the original outcome; the same key with a different request is rejected. Define the key's scope and retention period, and ensure concurrent requests using one key cannot both perform the side effect. A key alone is insufficient if recording it and charging the provider are not coordinated; use provider idempotency support and reconciliation for uncertain outcomes.

**Message-consumer example:** a consumer records a message or business-operation identifier in an inbox/deduplication store in the same transaction as its state change. Redelivery then does not apply that change twice. For external side effects, use the external provider's idempotency mechanism or a recoverable workflow; local deduplication cannot make a remote call atomic.

Idempotency is not the same as “the code ran once,” and it does not mean every repeated request must return the same HTTP status or response bytes. Define which business effect must happen at most once for a key and what a duplicate caller should observe. HTTP methods such as PUT are specified as idempotent in their intended effect, but POST operations and message handlers need application-level design when retries must be safe. See [API and protocols](../delivery-framework/03-api-protocols-and-endpoints.md), [data flow](../delivery-framework/04-data-flow.md), and [deep dives](../delivery-framework/06-deep-dives.md) for applying and testing it.

## Consistency and CAP

Consistency models describe what reads can observe relative to writes. CAP is a constrained result about behavior during a network partition, not a general rule that systems choose any two properties at all times. State the failure scenario and guarantees before applying it to a design.

See [data and storage](data-and-storage.md) and the Delivery Framework's [data-flow](../delivery-framework/04-data-flow.md) and [deep dives](../delivery-framework/06-deep-dives.md).
