# Event-Driven Data Platform Functional Specification

## Purpose and motivation

Teach how a system can accept data promptly while processing it asynchronously, preserving idempotency and making failures diagnosable. The example uses fictional scientific measurements and remains educational rather than production infrastructure.

## Stakeholders and actors

- **Submitting system/client:** sends a measurement batch and needs a prompt, unambiguous acceptance result.
- **Operator/learner:** follows a batch through processing, investigates failures, and reprocesses eligible work.
- **Analytics consumer:** reads the processed result from the analytical projection.

## Goals and non-goals

### Goals

- Accept valid batches without waiting for downstream processing.
- Process accepted batches asynchronously and persist an analytical result.
- Make retries, duplicate delivery, failure, and reprocessing understandable.
- Correlate a batch across the flow using stable identifiers.

### Non-goals

- Real clinical, patient, personal, or regulated data.
- Real Snowflake, production AWS, Kubernetes, or a production availability guarantee in the initial lab.
- Saga, Event Sourcing, or full CQRS without a demonstrated requirement.

## Use cases

1. A client submits a structurally valid batch with an idempotency key and correlation identifier.
2. The system accepts the batch and responds before analytical processing completes.
3. A worker processes accepted batches and writes an analytical result.
4. A transient failure is retried within a bounded policy.
5. An exhausted failure is recorded in a dead-letter path for operator inspection.
6. An operator reprocesses a failed batch without creating duplicate final results.
7. A consumer/learner traces batch status and processing signals.

## Business rules and boundaries

- One idempotency key represents one accepted submission.
- Repeated delivery must not create duplicate final analytical records.
- Acceptance means the platform accepted responsibility for processing; it does not claim processing is complete.
- Batch validation rejects invalid shape, types, or configured size limits before asynchronous processing.
- Data and examples remain synthetic.

## Acceptance criteria

- A valid new batch receives an acceptance result and becomes eligible for processing.
- Repeating an accepted request with the same idempotency key does not create a second batch.
- Processing can complete asynchronously and store one final result.
- A duplicate event does not duplicate the final projection.
- Exhausted transient failures are visible with batch/correlation context and can be reprocessed safely.

## Assumptions and unknowns

- The exact batch schema and maximum payload size must be specified by the implementation contract.
- The local lab targets and workload assumptions are listed in the [technical specification](technical.md); they are educational, not measured production commitments.
- No DAU is defined because the primary workload is system-to-system batch submission. Use batch rate, measurements per batch, payload bytes, burst profile, and consumer lag instead.
