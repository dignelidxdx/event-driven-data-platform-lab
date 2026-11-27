# Core Entities

Identify the important concepts the system must represent and the rules that must always hold. This step makes the problem domain explicit before committing to storage schemas or service boundaries.

For each concept, clarify its meaning, identity, lifecycle, relationships, ownership, and invariants. Separate entities with identity and lifecycle from value objects defined by their attributes. Use domain events to describe meaningful facts that have already occurred, with names in the past tense such as `OrderPlaced`.

## Modeling questions

- Who owns creation and state transitions? Which changes must be atomic?
- What is the entity's lifecycle, and which states and transitions are valid?
- Which fields are identifiers, business values, or derived data? What are the privacy and retention needs?
- What invariants must hold under retries or concurrent requests?
- What domain events describe completed business facts, and which consumers need them?

## Example concepts and measures

These are candidate measures to clarify product outcomes, not prescribed targets:

| Domain example | Concepts to model | Candidate business and quality measures |
|---|---|---|
| User / identity | User, credential reference, role, session | Sign-in success, authentication latency, account recovery time, unauthorized access rate |
| Payment | Payment attempt, authorization, capture, refund, provider reference | Authorization success, duplicate-charge rate, payment latency, reconciliation mismatch, unresolved payment age |
| Feed / tweet | User, post, follow, visibility, feed entry | Publish-to-visible delay, feed read latency, freshness, duplicate/missing post rate, moderation response time |
| Order | Order, line item, price snapshot, lifecycle state | Order creation success, time to accepted/rejected state, stuck-order age, cancellation and fulfillment rates |

Connect these measures to explicit SLIs/SLOs and definitions in [reliability and metrics](../concepts/reliability-and-metrics.md). Avoid using one aggregate availability number when critical workflows have different user impact.

## Consistency and quality choices

Decide which invariants require immediate consistency within a transaction and where eventual consistency is acceptable. For example, payment authorization must be protected from duplicate processing, while a feed projection may tolerate a short delay if the product accepts it. CAP is about guarantees during a network partition; it is not a blanket database selection rule. See [distributed systems](../concepts/distributed-systems.md) and [data and storage](../concepts/data-and-storage.md).

Useful outputs include a domain glossary, entity lifecycle/state diagrams, relationship sketch, event list, and explicit invariant examples. Use [DDD techniques](../concepts/architecture-patterns.md) to explore language and boundaries when helpful; do not infer microservices from the entity list.

DDD techniques can help discover a shared vocabulary, bounded contexts, and domain boundaries. They are tools for understanding the domain, not a requirement to split the application into services. See [architecture patterns](../concepts/architecture-patterns.md) and [distributed systems](../concepts/distributed-systems.md) for related concepts.

Record terms in a domain glossary and sketch relationships before mapping them to persistence. The model should explain the e-commerce flow in the lab: catalog items, cart contents, orders, identity, and payment responsibilities. Validate the vocabulary and rules with domain stakeholders.
