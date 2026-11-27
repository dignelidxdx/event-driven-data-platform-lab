# High-Level Design

Describe the major components, their responsibilities, boundaries, dependencies, and interactions. The design should make it possible to reason about change, failure, ownership, and quality attributes without hiding important trade-offs.

Start with the simplest architecture that satisfies the requirements. In this lab, the current design direction is an evolving modular monolith with explicit domain modules and ports at infrastructure boundaries. Consider service extraction only when requirements, ownership, or measured constraints justify its additional operational and consistency costs.

Use C4 context and container diagrams to communicate system boundaries and deployable units. Add component-level detail only where it answers a concrete question. Record significant choices and rejected alternatives in an [Architecture Decision Record](../artifacts-and-templates/architecture-decision-record.md); use an [RFC](../artifacts-and-templates/request-for-comments.md) to solicit feedback before a decision when appropriate.

## Choose a diagram for the question

| View | Useful for |
|---|---|
| C4 context/container/component | Stakeholder-to-system context, runtime/deployable units, and selected internal responsibilities |
| UML sequence or Mermaid sequence | Ordering and messages in one workflow |
| Domain model / class diagram | Domain concepts, relationships, and selected static structure |
| State machine | Valid lifecycle states and transitions, such as an order or payment |
| BPMN process diagram | Business process, participants, decisions, and handoffs |
| Data-flow diagram | Data stores, transformations, and trust boundaries |
| Deployment diagram | Runtime nodes, network zones, and deployment topology |
| ER diagram | Relational entities, keys, and cardinality |
| Cloud architecture diagram | Provider-specific services and infrastructure placement |

Mermaid is convenient for diagrams embedded in Markdown and reviewed with code. PlantUML supports text-based UML-style diagrams; Structurizr DSL models C4 views. Draw.io/diagrams.net and cloud-provider architecture icon sets can help with visual or provider-specific communication. Choose formats that preserve an editable source and render in the project's documentation environment. BPMN is a notation for business processes, not a replacement for component architecture views.

## Design for growth from evidence

Start with forecast workload and identify the bottleneck before adding infrastructure. If traffic rises, possible responses include query/index tuning, caching, vertical scaling (larger resources), and horizontal scaling (more instances). Horizontal scaling may require stateless application instances, shared or partitioned state, load balancing, and coordination changes.

Example progression for a read-heavy public feed: first measure request rate, p95/p99 latency, errors, cacheability, and database saturation; then consider a CDN for cacheable static/media or public responses, application caching for suitable reads, and a load balancer across stateless app instances. If database capacity remains limiting, investigate read replicas, partitioning, query redesign, or a projection/read model, each with consistency and operational trade-offs. A load balancer distributes traffic; it does not fix a saturated database or slow dependency.

For a small lab, these are design considerations, not a requirement to deploy a CDN, load balancer, broker, or microservices. Record the trigger for scaling and validate changes with representative load tests. See [capacity estimate](../artifacts-and-templates/capacity-estimate.md), [reliability review](../artifacts-and-templates/reliability-review.md), and [distributed systems](../concepts/distributed-systems.md).

Check that every component has a clear responsibility, dependencies point in intended directions, and the design can be traced back to requirements, entities, contracts, and data flows.
