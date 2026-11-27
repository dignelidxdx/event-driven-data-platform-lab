# Event-Driven Data Platform Model

An educational data-ingestion feature that accepts synthetic measurement batches and processes them asynchronously with traceable retries and failure handling.

This is a separate learning model in the repository. It is not an ingestion backend for the e-commerce or clinical-orders models unless a future integration exercise explicitly defines that relationship.

## Specifications

- [Functional specification](specs/functional.md) — actors, batch submission and processing use cases, business behavior, and acceptance criteria.
- [Technical specification](specs/technical.md) — domain model, workload assumptions, ports/adapters, events, local architecture, and cloud evolution options.

## Worked example

- [End-to-end walkthrough](../../examples/end-to-end/README.md) applies the [Delivery Framework](../../system-design/delivery-framework/README.md) through implementation/testing and a clearly labeled AWS deployment design.

## Status

The current specification defines a local educational lab. AWS deployment, production availability, Snowflake, and Kubernetes remain out of scope until separately approved.
