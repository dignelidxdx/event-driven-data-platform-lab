# Deep Dives

Use deep dives to examine design areas where uncertainty, scale, failure impact, or technical complexity could change the proposed solution. Select topics based on evidence and risk rather than expanding every area equally.

Typical topics include data ownership and transactions, consistency, partitioning, event delivery and replay, idempotency, backpressure, caching, capacity, security, privacy, observability, and recovery. Explain alternatives, assumptions, failure behavior, and consequences.

## What a deep dive contains

Choose a specific question, such as “What happens to order creation when the payment provider times out?” or “At what feed read rate does the database become the bottleneck?” State the relevant requirement, current design, assumptions, failure modes, options, evidence, decision, and remaining risks. Focus on a design choice that could materially change correctness, user experience, cost, or operations.

### Scaling and performance

Use workload estimates, profiling, load/stress/soak tests, latency percentiles, saturation signals, and cost measurements. Establish a baseline before tuning. Compare vertical scale-up against horizontal scale-out; test whether the application is stateless, whether dependencies and storage scale, and whether load balancing changes session or consistency behavior. Consider CDN/cache hit ratio, origin load, cache invalidation, and freshness. Do not infer capacity from a single average or synthetic request.

### Data and query behavior

Use query plans (`EXPLAIN` and, where safe, execution/analyze modes), representative data distributions, index analysis, migration rehearsal, and backup/restore tests. Evaluate transaction boundaries, isolation, contention, retention, partitioning, replay, and reconciliation. Understand production side effects before executing an analyze command.

### Distributed behavior and reliability

Draw sequence/state diagrams for timeout, retry, duplicate, out-of-order, and partial-failure paths. Evaluate idempotency, outbox/inbox, saga compensation, backpressure, circuit breaking, bulkheads, queue/stream lag, replay, and dead-letter recovery when relevant. Run fault-injection or dependency-stub experiments; document which guarantees are actually tested.

### Security and operability

Use a [threat model](../artifacts-and-templates/threat-model.md), dependency and configuration review, authorization tests, secret-handling checks, and data-flow review. Validate observability through traces, metrics, logs, alerts, dashboards, and runbooks. Exercise restore, rollback, and incident procedures against agreed RPO/RTO where appropriate.

## Example: growing feed traffic

Suppose feed requests increase while publish volume stays similar. Compare p95/p99 latency, request rate, cache hit ratio, database CPU/IO, query plans, connection saturation, and publish-to-visible freshness. A CDN may help cacheable media or public content; personalized feeds often need carefully scoped application caching or materialized read models. Add app instances behind a load balancer only if app capacity is the constraint and shared dependencies can sustain the added concurrency. Scale the database vertically as an initial operational option, then evaluate replicas or partitioning with explicit freshness and write-consistency costs. Load-test each change and define a rollback signal.

## Example: payment reliability

Trace order creation, payment authorization, timeout, retry, duplicate request, provider callback, and reconciliation. Test idempotency keys and state transitions, define retry limits and terminal/manual-review states, and measure authorization success, duplicate-charge incidents, provider latency, and time spent in pending state. Consider a saga only if the workflow spans independently committed steps and compensation is meaningful.

Useful supporting work may include a [capacity estimate](../artifacts-and-templates/capacity-estimate.md), [threat model](../artifacts-and-templates/threat-model.md), [reliability review](../artifacts-and-templates/reliability-review.md), [database query plan review](../artifacts-and-templates/database-query-plan-review.md), test report, and design review.

Validate critical assumptions with focused prototypes, experiments, or load tests. Summarize unresolved risks and connect outcomes to the [System Design Process validation stage](../system-design-process/08-validation-and-risk-review.md). Update relevant diagrams, contracts, and ADRs when findings change the design.
