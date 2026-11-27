# Delivery Framework

The Delivery Framework describes the technical sequence used to shape a solution in this lab. Start with requirements, make the domain and interfaces explicit, trace data movement, establish the architecture, and investigate the risks that need deeper analysis.

It is complemented by the [System Design Process](../system-design-process/README.md), which frames and communicates a system design from stakeholder problem through validation. This framework does not prescribe a language, framework, database, broker, or deployment topology; justify those choices from requirements and record consequential decisions.

## Stages

1. [Requirements](01-requirements.md)
2. [Core entities](02-core-entities.md)
3. [API, protocols, and endpoints](03-api-protocols-and-endpoints.md)
4. [Data flow](04-data-flow.md)
5. [High-level design](05-high-level-design.md)
6. [Deep dives](06-deep-dives.md)

## Supporting artifacts by stage

| Stage | Useful methods or artifacts |
|---|---|
| Requirements | [Requirements brief](../artifacts-and-templates/requirements-brief.md), workload assumptions, quality goals |
| Core entities | Domain model, glossary, and DDD techniques where useful |
| API, protocols, and endpoints | [API and event contract](../artifacts-and-templates/api-and-event-contract.md), sequence diagrams, RFC for broad review |
| Data flow | Data-flow diagram, event schemas, C4 diagrams where they clarify boundaries |
| High-level design | C4 context/container diagrams; ADRs for significant decisions |
| Deep dives | Capacity estimate, threat model, reliability review, query-plan analysis with `EXPLAIN` |

These artifacts are selected according to the system's risks and audience; they are not required deliverables for every change. See the [System Design Process](../system-design-process/README.md) for the end-to-end design workflow and stakeholder review.

## Example scale-up reasoning

When user and request counts grow, return to the workload and quality targets first. Determine whether latency or errors correlate with application CPU, database saturation, dependency limits, network, or cache misses. Then choose a response: optimize the constrained path, scale vertically, or add horizontal instances. Horizontal app scaling usually needs load distribution and safe shared-state handling. A CDN can reduce origin traffic for cacheable assets or responses; it does not accelerate personalized or frequently changing data without an explicit freshness policy. Read replicas, partitioning, queues, and stream processing address different constraints and introduce their own consistency and operational trade-offs.

The [high-level design](05-high-level-design.md) stage records candidate topology; [deep dives](06-deep-dives.md) defines evidence and experiments to validate it.
