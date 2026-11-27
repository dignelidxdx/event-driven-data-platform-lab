# Event-Driven Data Platform Lab — B1 Design

A Python lab for preparing for a Principal Software Engineer interview while building extensible foundations for event-driven data architecture. The goal is to demonstrate justified decisions, not to simulate production complexity.

## Problem and scope

A fictional platform receives batches of scientific data, validates them, processes them asynchronously, and records a simulated analytical result. The visible path is: `API → event → worker → analytical storage`, with idempotency, retries, a DLQ, and traceability.

| In scope | Out of scope |
|---|---|
| HTTP API, validation, ingestion, events, workers, SQL, and basic observability | Deployed microservices, real Snowflake, real AWS, Kubernetes, and personal or clinical data |
| Local simulation of a queue, DLQ, and analytical storage | Saga, Event Sourcing, and complete CQRS |
| Hexagonal architecture and lightweight DDD | Processing large files or guaranteeing production availability |

Different research systems deliver measurement batches to a central platform. Processing may fail or take time; blocking the API until it finishes would make the integration fragile. The lab must accept the batch quickly, process it without coupling the client to the worker, and retain enough evidence to investigate failures.

## Assumptions and learning SLOs

| Assumption | Guiding decision |
|---|---|
| Peak of 50 batches/minute; each batch has up to 10,000 measurements | Asynchronous processing and bounded batch size |
| An operator needs to follow a batch through the flow | `correlation_id`, structured logs, and metrics |
| The analytical layer may be temporarily unavailable | Decoupled acceptance and eventual consistency |

### Educational SLOs

- 99% of valid requests receive acceptance in less than 500 ms without waiting for processing.
- No retry of the same key creates more than one final analytical record.

## Architecture decision

| Approach | Advantage | Cost | Decision |
|---|---|---|---|
| Synchronous monolith | Simple to run | Couples the API to processing failures and latency | Rejected |
| Microservices from the start | Makes physical boundaries visible | Adds network, deployment, and operational cost before justification | Rejected |
| Modular monolith with ports and local events | Practices boundaries, events, and testing without premature infrastructure | Physical separation remains a later evolution | Chosen |

## Modules and contracts

| Module | Responsibility | Event |
|---|---|---|
| Ingestion | Accept and validate batches | `BatchAccepted` |
| Processing | Process measurements and publish results | `BatchProcessed` |
| Analytics | Store processed output in a local SQL projection | `ProcessedBatchStored` |
| Operations | Expose metrics, logs, and traces; manage the DLQ | `BatchProcessingFailed` |

- **Domain:** `ScientificBatch` and `Measurement` entities, validation rules, and batch states.
- **Application:** `AcceptBatch`, `ProcessBatch`, `StoreProcessedBatch`, and `ReprocessFailedBatch` use cases.
- **Ports:** idempotency repository, event publisher/consumer, analytical store, clock, and telemetry.
- **Adapters:** FastAPI, local queue implementation, SQLite, and local logging/metrics. AWS/SQS/SNS and Snowflake adapters are extensions, not initial requirements.

## Main flow

1. The client sends a batch with `idempotency_key` and `correlation_id`.
2. Ingestion validates the contract and atomically records acceptance with the key.
3. Ingestion publishes `BatchAccepted`.
4. Processing consumes the event and publishes `BatchProcessed`.
5. Analytics persists a local SQL projection and marks the batch complete.
6. On a transient failure, the consumer retries with a limit. When exhausted, it publishes the message and reason to the DLQ.

## Invariants

- An idempotency key represents one accepted batch.
- Consumers are idempotent: a repeated event does not duplicate a final projection.
- Events contain an identifier, version, date, `correlation_id`, and the minimum required data.
- The client receives acceptance; it does not receive a false promise of immediate processing.
- Lab data is synthetic: no clinical, personal, or regulated data is included.

## Security and ethics

- Validate size, schema, and types before enqueueing.
- Keep secrets out of the repository; use environment configuration only when implemented.
- Initially use simulated authentication to focus on boundaries and flows; real IAM/OAuth is an explicit evolution.
- Do not model patients or medical decisions: measurements are fictional scientific data and are not actionable.

## Observability

| Signal | Use |
|---|---|
| Structured log with `correlation_id`, `batch_id`, and state | End-to-end batch diagnosis |
| Acceptance and processing latency histogram | Evaluate educational SLOs |
| Queue and DLQ depth | Detect saturation or persistent failure |

## Deliberate evolution

1. Replace the local queue with SQS/SNS without changing use cases, through the messaging port.
2. Replace SQLite with Snowflake or a compatible staging layer behind the analytical port.
3. Separate Processing when scale, deployment autonomy, or failure profiles justify it.
4. Evaluate orchestration/Saga only when a distributed transaction with real compensations exists.

## Interview discussion

- **Why not microservices:** boundaries are explicit, but the load and team do not yet justify the operational cost.
- **How duplicates are handled:** idempotency at ingestion and consumption; at-least-once delivery does not imply duplicate results.
- **How failures are recovered:** bounded retries, an observable DLQ, and explicit reprocessing.
- **What would change on AWS:** API Gateway/Lambda or ECS for ingestion, SQS/SNS for events, CloudWatch/OpenTelemetry for signals, least-privilege IAM, and Snowflake as the analytical destination.

## Approval criteria before implementation

- [ ] The problem, boundaries, and assumptions are understandable without code.
- [ ] Each component has one responsibility and an identifiable contract.
- [ ] Evolution toward AWS/Snowflake does not force changes in the domain or use cases.

## Next step

Review and approve this design. Then define the repository structure, initial ADRs, and container diagram before implementation.
