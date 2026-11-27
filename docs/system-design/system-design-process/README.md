# System Design Process

This process helps a team move from a stakeholder problem to a validated and communicable system proposal. It complements the [Delivery Framework](../delivery-framework/README.md): that framework organizes the technical solution through requirements, core entities, interfaces, data flow, high-level design, and deep dives; this process frames the design work, makes trade-offs explicit, validates risk, and prepares a stakeholder walkthrough.

Use the depth appropriate to the change. A small feature may need a short requirements note and one diagram. A high-scale or high-risk system may need workload estimates, multiple alternatives, detailed failure analysis, and review artifacts. The [System Design Document](../artifacts-and-templates/system-design-document.md) can serve as the evolving proposal.

## Stages

1. [Problem and scope](01-problem-and-scope.md)
2. [Requirements and workload](02-requirements-and-workload.md)
3. [Domain and data](03-domain-and-data.md)
4. [Capacity and quality attributes](04-capacity-and-quality-attributes.md)
5. [Options and trade-offs](05-options-and-trade-offs.md)
6. [High-level architecture](06-high-level-architecture.md)
7. [Deep dives](07-deep-dives.md)
8. [Validation and risk review](08-validation-and-risk-review.md)
9. [Stakeholder walkthrough](09-stakeholder-walkthrough.md)

## Supporting artifacts by stage

| Stage | Supporting methods or artifacts |
|---|---|
| Problem and scope | Requirements brief, stakeholder notes |
| Requirements and workload | System Design Document, workload assumptions, capacity estimate |
| Domain and data | Domain model, glossary, data model; DDD techniques where useful |
| Options and trade-offs | RFC for broad discussion before a decision; ADR to record a significant decision and its consequences |
| High-level architecture | C4 context and container diagrams; component diagrams when useful |
| Deep dives | API/event contracts, sequence diagrams, threat model, reliability review, database query-plan review with `EXPLAIN` |
| Validation and risk review | Prototypes, load tests, risk register, design review checklist |
| Stakeholder walkthrough | Concise design summary, diagrams, decisions, open questions |

These are optional aids, chosen to answer concrete questions. C4 is a diagramming model, ADR records a decision, RFC invites discussion, DDD offers domain modeling techniques, and `EXPLAIN` inspects a database query plan; none is a complete system-design process on its own.
