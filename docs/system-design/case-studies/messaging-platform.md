# Case Study: Messaging Platform

## Scenario

Design a messaging platform for one-to-one and group conversations. This is a generic system-design exercise, not a description of WhatsApp or another company's implementation.

## Questions and requirements

- Are messages text-only or do they include media, edits, reactions, and delivery receipts?
- What delivery, ordering, retention, offline, and multi-device behavior is required?
- What are concurrent connections, message rates, group sizes, and regional needs?
- What privacy, abuse prevention, account recovery, and encryption requirements apply?

## Design path

1. Define send, receive, history, membership, and delivery behavior with measurable quality goals.
2. Model conversations, participants, messages, devices, and delivery state with explicit ownership rules.
3. Specify send, synchronization, acknowledgement, and media upload contracts.
4. Trace message acceptance, durable storage, routing, offline delivery, and acknowledgement.
5. Compare connection management, partitioning, persistence, and media storage options against estimated workload.
6. Deep dive on ordering scope, duplicate delivery, retries, backpressure, multi-region behavior, retention, and recovery.

## Candidate trade-offs

At-least-once delivery with idempotent processing can simplify recovery but requires duplicate handling. Stronger ordering guarantees can constrain parallelism and partitioning. Long-lived connections improve server-initiated delivery but require connection lifecycle and capacity management. Security and encryption choices affect key management, search, recovery, and moderation capabilities; define them from product and threat requirements.

Use sequence/data-flow diagrams for delivery and reconnect behavior, a threat model for trust boundaries, and capacity estimates for connection and message workloads. Validate recovery, burst behavior, and delivery semantics with failure and load experiments.
