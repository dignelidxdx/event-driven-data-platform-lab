# High-Level Architecture

Describe the system boundary, major components, responsibilities, ownership, external dependencies, and the interactions that implement important workflows. Show where data is authoritative and where asynchronous boundaries exist.

Use C4 context and container diagrams to communicate the system and its major runtime units. Add component diagrams or sequence diagrams only when they resolve a design question. Link each diagram to requirements and the [Delivery Framework high-level design](../delivery-framework/05-high-level-design.md).

Explain how the structure supports the quality goals and identify its main trade-offs. Record consequential decisions in ADRs and keep the diagrams consistent with API and event contracts.

## Keep architecture connected to implementation

Treat architectural decisions as working hypotheses about how to meet the requirements and quality goals. Develop in small increments with the implementation team, using code, tests, reviews, or focused prototypes to validate those decisions. Findings from implementation—such as an unexpected dependency, failure mode, or delivery cost—should return to the architecture discussion and may change the design. Update the ADR and related diagrams or contracts when a consequential decision changes; ordinary implementation details do not need their own ADR. Share critical-path work and review responsibilities so architectural guidance helps the team move without making one person a bottleneck. See the [engineering workflow](../engineering-workflow/README.md) for collaboration and review practices.
