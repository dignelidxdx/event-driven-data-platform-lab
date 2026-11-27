# Code and Testing Path

The existing event-driven data platform design describes a Python lab using FastAPI, a local queue adapter, SQLite, and explicit ports. Follow that specification for the local implementation. The AWS mapping is a later adapter/deployment exercise, not a reason to change the domain model.

## Suggested implementation sequence

1. Create domain types and validation rules for `ScientificBatch`, `Measurement`, and batch state transitions.
2. Implement `AcceptBatch` with idempotency behind a repository port.
3. Define versioned `BatchAccepted`, `BatchProcessed`, and failure contracts.
4. Implement event publisher/consumer ports and local adapters.
5. Implement `ProcessBatch`, bounded retries, dead-letter behavior, and idempotent projection storage.
6. Expose the HTTP adapter and contract documentation.
7. Add structured logging and metrics keyed by batch/correlation identifiers.

## Test pyramid for the example

| Test level | Example checks |
|---|---|
| Domain unit | Invalid measurement is rejected; state transition is legal; repeated processing does not duplicate a result |
| Use-case unit | Same idempotency key returns the prior acceptance; transient errors follow retry policy |
| Adapter/integration | HTTP contract maps to use case; local queue delivers and retries; SQLite projection persists expected output |
| Contract | Event schema compatibility and producer/consumer expectations remain aligned |
| End-to-end | Submit a batch, observe acceptance, process it, and query the final result; force a failure and verify DLQ/reprocessing path |
| Load/failure experiment | Exercise representative batch sizes and bursts; observe latency, queue age, worker saturation, and recovery behavior |

Tests should assert observable behavior and invariants rather than implementation details. Keep synthetic data only. For AWS adapters, add environment-isolated integration tests and verify permissions, retry behavior, and resource cleanup before considering deployment.
