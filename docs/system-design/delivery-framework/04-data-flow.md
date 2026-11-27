# Data Flow

Trace how information enters, changes, moves through, and leaves the system. Show which component owns each step and where durable state or externally visible effects occur.

For each important workflow, identify the trigger, sequence of reads and writes, emitted events, consumers, transformations, failure points, and retry or deduplication behavior. Distinguish synchronous calls from asynchronous delivery and make ordering or consistency assumptions explicit.

## Synchronous and asynchronous movement

- **Request/response:** caller waits for a result; useful when the user needs an immediate answer, but couples latency and availability to the dependency chain.
- **Queue-based messaging:** a message is handed to a consumer for work; supports buffering and retries, but requires idempotency, dead-letter handling, and visibility into queue age.
- **Event streaming:** ordered records are retained for a period and consumers can track offsets or replay; useful for multiple consumers and continuous data movement, with partitioning, schema, retention, and lag decisions.
- **Change Data Capture (CDC):** captures database changes from logs or another mechanism and streams them downstream. For example, committed order changes can be captured and delivered to an analytics warehouse such as BigQuery. Define whether the source of truth is the database, how deletes/schema changes are represented, and how lag, duplicates, and replay are handled.
- **Webhooks:** deliver notifications to external consumers; plan for retries, signatures, delivery records, and duplicate events.

Do not confuse an event stream with a task queue: retention, replay, ordering, and consumer behavior can differ. A transactional outbox can help atomically persist business state and a message-to-publish record; CDC may then relay the outbox. These options add infrastructure and should be justified by delivery guarantees.

## Describe and inspect the flow

Use a data-flow diagram for trust boundaries and data stores, a sequence diagram for time-ordered interactions, and an event-storming/domain event map during discovery. Mermaid can keep simple flow, sequence, and state diagrams versioned in Markdown. For larger or stakeholder-facing diagrams, use a dedicated diagram source (for example, PlantUML, Structurizr DSL, or a cloud architecture diagram tool) and link the editable source. Record event names, schema ownership, keys, ordering scope, delivery semantics, retention, and consumer lag expectations.

For a CDC-to-warehouse design, show the transaction commit, log capture, connector/stream, transformation, warehouse write, deduplication key, schema evolution, and freshness target. Test backfill and recovery as well as the steady-state path.

Use a data-flow or sequence diagram when prose obscures the path. C4 diagrams can show system context and containers; event schemas and API contracts describe the payload boundaries. Link diagrams to the workflow requirements and [API and event contracts](../artifacts-and-templates/api-and-event-contract.md).

For the lab's order flow, trace catalog and cart interactions through order creation, publication of `OrderPlaced`, projection updates, and the payment port. Keep the diagram aligned with implemented behavior as the lab evolves.
