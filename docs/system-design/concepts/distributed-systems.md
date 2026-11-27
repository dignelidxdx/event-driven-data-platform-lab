# Distributed Systems

Distributed systems coordinate work across processes or machines. Network delay, partial failure, concurrent updates, and independent deployment affect correctness and operations. Introduce these costs when they solve a real requirement.

## Event-driven architecture

Producers publish facts or requests that consumers process asynchronously. This can reduce temporal coupling and support multiple consumers, while making delivery, ordering, replay, observability, and eventual consistency explicit concerns. Name domain events as past-tense facts.

**Consider event-driven architecture when** producers and consumers need independent lifecycles, one business fact has multiple downstream uses, or work can complete asynchronously. Define delivery guarantees, event ownership and schema evolution, ordering needs, duplicate handling, replay policy, and acceptable propagation delay. A direct synchronous call is usually simpler when one caller needs an immediate result from one owner.

### Fire-and-forget

Fire-and-forget means a caller starts work and does not wait for its completion or outcome. It is suitable only when the caller does not need the result to decide its response and the work may safely finish later (for example, best-effort telemetry). Returning after an in-memory task is started does not make the work durable: process failure can lose it, and errors may go unobserved.

For business effects such as sending an order confirmation, use a durable queue or an outbox-backed event when loss is unacceptable. Define acknowledgement, retry, idempotency, dead-letter or reconciliation behavior, and monitoring. If the caller needs to know whether the operation succeeded, expose an explicit result or accepted/pending state instead of fire-and-forget.

## CQRS

Separates command handling that changes state from query models optimized for reads. CQRS can help when read and write needs differ materially; separate models and synchronization are unnecessary complexity for many applications.

**Consider CQRS when** read traffic or query shapes differ substantially from write rules, read models need independent optimization, or commands must enforce explicit domain behavior. Start with separate code paths in one application if that solves the problem; separate databases and asynchronous projections add lag, reconciliation, and operational work and should be justified by requirements.

## Event sourcing

Stores state changes as an append-only event history and derives current state by replay or projection. It supports audit and temporal reconstruction when those are core needs, while increasing schema evolution, replay, projection, and operational complexity. It is not a prerequisite for event-driven architecture.

**Consider Event Sourcing when** the authoritative history of business decisions is itself required (for example, regulated audit or reconstructing how an order reached its current state), and the team can version events and operate projections and replays. It is not simply a way to publish events. If current state plus ordinary audit records meets the requirement, conventional persistence is usually simpler.

## Saga

### The problem it solves

Suppose checkout changes order, inventory, and payment data owned by three separate services. A normal database transaction can atomically commit or roll back changes in one database, but it usually cannot wrap all three independently owned databases in one practical ACID transaction. If the order is created and inventory is reserved, then the payment provider times out, the system is left with completed work and an uncertain next step. Simply returning an error does not undo the order or release the stock.

A **Saga** makes this multi-service workflow explicit. It runs a sequence of local transactions, each committed by the service that owns that data. If a later step cannot complete, the workflow runs business actions that compensate for earlier completed steps. This provides a controlled path to finish, compensate, or escalate the workflow without pretending all services shared one atomic transaction.

### Checkout example

1. Create the order in `Pending` state.
2. Reserve inventory. The inventory service commits its own reservation.
3. Authorize payment. The payment provider commits its own authorization.
4. Confirm the order and request shipment.
5. If shipment cannot be arranged, mark the order as unable to proceed, release the inventory reservation, and void the payment authorization (or refund it if it was already captured).

The final actions are **compensations**, not database rollbacks: a released reservation and a payment refund are new business events, and the original actions may remain visible in audit records. If a compensation also fails, retry it safely and expose the workflow for reconciliation or manual intervention. Keep explicit state so a timeout can be distinguished from a confirmed failure; a timeout may mean the remote action succeeded but its reply was lost.

### When to consider it

Consider a Saga when one business outcome requires changes across independently committed services or resources, a single local transaction cannot cover them, and the product needs defined behavior for partial completion or failure. Before adopting one, confirm that the workflow really crosses ownership/transaction boundaries; list each step, timeout, retry, duplicate-handling rule, compensation, terminal state, and manual-recovery path. Make steps and compensations idempotent where retries are possible.

Do **not** add a Saga for several writes that can safely live in one database transaction. It adds states, retries, compensation logic, monitoring, and recovery work. If the business cannot compensate an action (for example, a shipment already delivered), define an alternative such as a refund, replacement, or human review; compensation does not always restore the exact original world.

An **orchestrated Saga** uses a coordinator to decide and record the next step, which makes workflow state and progress visible in one place. A **choreographed Saga** has services react to events and emit further events, which avoids a central coordinator but can make the full workflow harder to trace. Choose based on the number of steps, need for central visibility, and team ability to observe and operate the flow.

## Idempotency

An operation is idempotent when repeating the same request or message produces the same intended business outcome as processing it once. This matters because clients, brokers, and workers retry after timeouts or uncertain acknowledgements: the first attempt may have succeeded even though its response was lost. Idempotency prevents a retry from creating a second order, charging a customer twice, or applying the same event twice.

**Consider explicit idempotency controls whenever** a caller or transport may retry a state-changing operation, delivery is at least once, or a timeout can leave the result uncertain. Decide the operation identity, key scope, retention period, atomic recording mechanism, behavior for key reuse with a different payload, and what duplicate callers receive. For naturally idempotent state-setting operations, document and test that property; do not add a key store without a retry or duplicate-effect risk.

**Payment API example:** the client sends a stable idempotency key with a payment request. The server atomically records the key, request identity, and outcome with the payment operation. A retry with the same key and same request returns the original outcome; the same key with a different request is rejected. Define the key's scope and retention period, and ensure concurrent requests using one key cannot both perform the side effect. A key alone is insufficient if recording it and charging the provider are not coordinated; use provider idempotency support and reconciliation for uncertain outcomes.

**Message-consumer example:** a consumer records a message or business-operation identifier in an inbox/deduplication store in the same transaction as its state change. Redelivery then does not apply that change twice. For external side effects, use the external provider's idempotency mechanism or a recoverable workflow; local deduplication cannot make a remote call atomic.

Idempotency is not the same as “the code ran once,” and it does not mean every repeated request must return the same HTTP status or response bytes. Define which business effect must happen at most once for a key and what a duplicate caller should observe. HTTP methods such as PUT are specified as idempotent in their intended effect, but POST operations and message handlers need application-level design when retries must be safe. See [API and protocols](../delivery-framework/03-api-protocols-and-endpoints.md), [data flow](../delivery-framework/04-data-flow.md), and [deep dives](../delivery-framework/06-deep-dives.md) for applying and testing it.

## Optimistic concurrency and compare-and-swap

Optimistic concurrency control lets concurrent work proceed without holding a lock for the whole read-edit-write interval. Store a version or other concurrency token with the record; on update, atomically write the new state only if the stored token still equals the one that was read. This conditional update is compare-and-swap (CAS). If another writer changed the record first, the update affects no row (or returns a conflict), and the caller must reload and retry, merge, or report the conflict according to business rules.

For example, read an order with version 7, compute a change, then update it with a condition equivalent to `WHERE id = ? AND version = 7`, incrementing the version in the same atomic database operation. A snapshot is the data read at a point in time; it is not itself concurrency control. CAS over a version can prevent a stale snapshot from overwriting newer state. Database-specific mechanisms such as a conditional update, ETag/`If-Match`, or a compare-and-set primitive can provide the condition; the comparison and write must be atomic.

**Consider optimistic concurrency when** conflicts are possible but relatively uncommon, operations are short, and stale writes must be detected without serializing every reader and writer. It is common in database-backed applications and APIs that use version columns or entity tags. It does not prevent conflicts; choose a bounded retry policy and decide whether to merge or return a conflict. For frequent contention, multi-record invariants, or operations that cannot be safely retried, use an appropriate transaction, locking, or domain-specific coordination strategy. A version check is only effective if every relevant writer follows it.

## Consistency and CAP

Consistency models describe what reads can observe relative to writes. CAP is a constrained result about behavior during a network partition, not a general rule that systems choose any two properties at all times. State the failure scenario and guarantees before applying it to a design.

See [data and storage](data-and-storage.md) and the Delivery Framework's [data-flow](../delivery-framework/04-data-flow.md) and [deep dives](../delivery-framework/06-deep-dives.md).
