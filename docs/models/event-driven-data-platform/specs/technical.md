# Event-Driven Data Platform Technical Specification

## Status and traceability

This document describes the local educational implementation direction. The [functional specification](functional.md) is authoritative for user-visible behavior. AWS, Snowflake, and production operation are possible future evolutions, not current implementation commitments.

## Domain model

- `ScientificBatch`: accepted unit of submitted work with identity and lifecycle.
- `Measurement`: synthetic measurement belonging to a batch.
- `ProcessingAttempt`: one execution attempt and its outcome.
- Analytical result/projection: processed representation consumed by analytics.

Candidate batch states are Accepted, Processing, Processed, and Failed. The implementation must define allowed transitions, retry state, and whether reprocessing creates a new attempt or changes batch state. Domain events are past-tense facts such as `BatchAccepted` and `BatchProcessed`.

### Invariants

- An idempotency key maps to at most one accepted batch within its defined retention period.
- Consumers are idempotent: repeated delivery does not duplicate the final analytical record.
- Events include identifier, schema version, timestamp, `correlation_id`, and only required payload data.
- The acceptance response does not represent completion of asynchronous processing.

## Workload and capacity assumptions

The existing educational design assumes a peak of 50 batches/minute and up to 10,000 measurements per batch. Treat these as scenario inputs, not measured traffic or a production capacity guarantee.

The workload is machine-to-machine batch ingestion, so DAU/MAU is not the primary capacity measure. Before selecting technology or sizing infrastructure, record:

| Measure | Current status | Why it matters |
|---|---|---|
| Batches per minute / peak burst | Educational assumption: 50/minute; burst shape unknown | API concurrency, queue ingress, and worker scaling |
| Measurements per batch | Educational maximum: 10,000; bytes per measurement unknown | Payload limits, memory, storage, and processing time |
| Batch payload bytes | Unknown | HTTP limits, object-storage need, and transfer cost |
| Processing duration and CPU/memory | Unknown; measure with representative synthetic batches | Worker count and backlog recovery time |
| Retention and replay window | Unknown | Storage cost, idempotency retention, and reprocessing design |
| Analytics freshness target | Unknown beyond eventual consistency | Queue/stream choice and consumer-lag objective |
| Availability, RPO, and RTO | Not production-defined | Redundancy, backup, and recovery design |

Do not convert the 99% under 500 ms educational acceptance target into a production SLO without representative measurement, an agreed time window, eligible request definition, and owner.

## Quality attributes and candidate metrics

- Acceptance: valid request rate, rejection reasons, p95/p99 acceptance latency.
- Processing: accepted-to-processed duration, throughput, error/retry rate, and stuck-batch age.
- Messaging: queue depth/age, consumer lag, duplicate suppression, DLQ depth/age.
- Data integrity: batches with exactly one final projection, reconciliation mismatches, and idempotency conflicts.
- Operations: time to detect and recover a processing failure, with definitions agreed by the team.

Use precise units, time windows, and data sources. Metrics are candidate indicators, not targets until stakeholders set thresholds.

## API and event contracts

The client submits an HTTP batch request with an idempotency key and correlation identifier. The exact endpoint, schema, authentication, error model, size limits, and status-query behavior remain to be finalized in the [API and event contract](../../../system-design/artifacts-and-templates/api-and-event-contract.md). Events include at least `BatchAccepted`, `BatchProcessed`, and `BatchProcessingFailed`; versioning and compatibility rules must be explicit.

## Data flow and architecture

```text
HTTP adapter → AcceptBatch use case → idempotency repository
             → event publisher → local queue → ProcessBatch use case
             → analytical store/projection
```

The chosen learning architecture is a modular application with ports for idempotency storage, event publication/consumption, analytical storage, clock, and telemetry. Local adapters are FastAPI, a simulated queue, SQLite, and local logs/metrics. Keep domain and use cases independent from these tools.

Avoid claiming atomic acceptance plus event publication unless the implementation provides a transaction/outbox or equivalent guarantee. If a local simulation cannot provide that guarantee, document the gap and test its failure behavior.

## Technology decision

The existing design names Python, FastAPI, and SQLite for the local learning adapter. They are selected to keep the initial exercise executable and small, not because workload measurements prove they are optimal. The local queue simulates asynchronous behavior. AWS SQS/SNS and an analytical warehouse are optional adapters for a separately scoped cloud exercise.

Before changing technologies, compare runtime and build workflow, ecosystem, team learning goals, expected workload, deployment model, observability, cost, and operational burden. See [technology selection](../../../system-design/technology/README.md). Record consequential choices and alternatives in ADRs.

## Security and privacy

- Use synthetic, non-clinical data only.
- Validate schema, types, and size at the input boundary.
- Keep credentials out of source control.
- Simulated authentication is acceptable for the local exercise; real identity, IAM, authorization, and key management require a separate design.
- Do not log full batch payloads or sensitive values; correlate using identifiers.

## Failure handling and recovery

Define bounded retry policy, duplicate behavior, poison-message handling, DLQ visibility, operator reprocessing, and analytical reconciliation. A production design must add explicit retention, backup, restore, RPO/RTO, and replay policy.

## Validation plan

- Domain tests for batch validation, state transitions, and invariants.
- Use-case tests for idempotent acceptance, retries, and duplicate processing.
- Adapter/integration tests for HTTP, queue, and SQL projection behavior.
- Contract tests for event compatibility.
- End-to-end test from batch submission to final projection, including a failure/DLQ path.
- Representative load and recovery experiments before making capacity or production SLO claims.

## Open decisions

- Exact batch schema, payload cap, idempotency-key scope, and retention.
- Acceptance response and whether a status endpoint is needed.
- Event publication atomicity and local queue semantics.
- Processing duration, analytics target/freshness, and recovery expectations.
- Whether and when an AWS deployment is in scope.
