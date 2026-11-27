# Delivery Framework Walkthrough

This walkthrough applies each of the six [Delivery Framework stages](../../system-design/delivery-framework/README.md) to the batch-ingestion example.

## 1. Requirements

Separate functional behavior from quality attributes. The client needs acceptance, not a false promise that the batch is already processed. Capture the educational latency and throughput assumptions, then ask what peak bursts, payload limits, retention, and recovery commitments a real stakeholder requires.

Use example-based requirements or BDD scenarios to clarify acceptance and duplicate behavior. TDD can guide implementation of a specific invariant; it does not replace the product specification. Link acceptance tests to the cases in [the specification walkthrough](01-specification.md).

## 2. Core entities

Model `ScientificBatch`, `Measurement`, and `ProcessingAttempt`. Define states such as Accepted, Processing, Processed, and Failed, plus valid transitions and ownership. `idempotency_key` identifies a request for acceptance; `batch_id` identifies the accepted domain object; `correlation_id` connects telemetry across the flow.

Candidate measures include acceptance latency, accepted-to-processed duration, processing success/failure rate, retry count, duplicate suppression count, queue age/depth, and DLQ age/depth. Define units, aggregation windows, and sources before setting operational targets. A duplicate event must not duplicate the final analytical result.

## 3. API, protocols, and endpoints

Use HTTP request/response for client submission because the client needs a prompt acceptance result. A candidate endpoint is `POST /batches`; return a stable batch identifier and an accepted status. Decide whether a status resource such as `GET /batches/{batchId}` is needed by the product; it is not required by the current base flow.

Describe request, response, validation errors, authentication boundary, idempotency behavior, and size limits in OpenAPI. Document the asynchronous event separately, including schema version, batch identifier, correlation identifier, delivery semantics, and compatibility. Use contract tests for producer/consumer compatibility.

## 4. Data flow

Trace request validation → durable acceptance/idempotency record → event publication → worker processing → analytical projection write → completion status. For each edge, document ownership, retries, duplicate behavior, ordering assumptions, and failure visibility.

The local lab may simulate a queue. A future AWS variant could use a durable queue and dead-letter configuration, but the message must remain a notification of accepted work with a stable identifier and schema contract. Consider an outbox if atomicity between state persistence and event publication is required by the chosen implementation.

## 5. High-level design

Keep domain and use cases independent of transport, queue, and storage. The existing design chooses a modular monolith with ports and local adapters. Draw C4 context and container views that show the submitting client, platform boundary, ingestion/processing responsibilities, and analytical destination. Add a sequence diagram for retry and failure paths.

For the AWS variant, map responsibilities to candidate managed services in [AWS architecture by category](03-aws-architecture-by-category.md). Record each binding choice and alternative in an ADR.

## 6. Deep dives

Prioritize idempotency and duplicate delivery, durable acceptance, retry/DLQ/reprocessing, data volume and batch-size limits, least-privilege access, analytics freshness, and operational diagnosis. Use load/failure tests and cost estimates to validate assumptions. Define production RPO/RTO and SLOs only after stakeholder and operational review.

The output of this stage is a small set of evidence-backed design decisions, unresolved risks, and validation tasks—not a list of every possible AWS service.
