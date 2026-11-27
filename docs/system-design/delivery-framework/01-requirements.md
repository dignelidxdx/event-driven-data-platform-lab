# Requirements

Requirements state what the system must do and the conditions under which it must do it. They guide domain modeling, interface design, architecture, and validation.

## Functional requirements

Describe observable capabilities as user or system outcomes. Keep them testable and connect each to an actor or trigger. In the lab, examples include browsing a catalog, adding a product to a cart, confirming a cart, and creating an order.

### Example: order and payment

- A customer can submit a valid cart and receive an order identifier.
- The system records the order state and publishes `OrderPlaced` only after the order is accepted.
- A payment failure leaves the order in a defined state and can be retried or compensated according to an explicit policy.

### Specification and development approaches

- **Specification-Driven Development (SDD):** express expected behavior in a reviewed specification before or alongside implementation, then keep the specification and implementation aligned. Use it when multiple people or components need a shared source of truth. The term has multiple interpretations, so state the project workflow explicitly.
- **Test-Driven Development (TDD):** write a failing behavior-focused test, implement the smallest change, and refactor. It helps clarify examples and domain invariants; it does not replace stakeholder requirements or system-level validation.
- **Behavior-Driven Development (BDD):** express examples in stakeholder-readable scenarios, often using Given/When/Then. It can bridge product behavior and acceptance tests; avoid turning every internal detail into a brittle scenario.
- **Example Mapping / Three Amigos:** clarify a story through rules, examples, and questions with product, development, and testing perspectives before implementation.
- **ATDD (Acceptance Test-Driven Development):** agree on acceptance examples before building, then automate suitable checks.

These approaches can be combined. A practical sequence is a concise requirements brief, concrete examples, an agreed specification, and focused tests; choose the rigor according to uncertainty and risk.

## Non-functional requirements

Describe measurable quality attributes and constraints, such as latency, availability, throughput, durability, consistency, security, privacy, operability, and cost. Give each a measurement context and target where known. If a value is unknown, record it as an assumption to validate rather than presenting it as a commitment.

### Turn vague goals into scenarios

Instead of “the feed must be fast,” state the operation, expected load, percentile and time window, and target once stakeholders provide them. For example: “At the forecast peak read rate, 99% of feed requests complete within the agreed latency target.” Do not invent the target; validate it against user impact and cost.

### Supporting methods and documents

Start with a [requirements brief](../artifacts-and-templates/requirements-brief.md). Use a [System Design Document](../artifacts-and-templates/system-design-document.md) as the proposal that connects requirements, workload, architecture, and validation. A short RFC is useful when several stakeholders need to compare approaches. A quality-attribute workshop or scenario review can make latency, availability, consistency, recoverability, security, and cost trade-offs explicit.

## Capture and validation

Record scope, users, workflows, exclusions, dependencies, constraints, assumptions, and success measures in a [requirements brief](../artifacts-and-templates/requirements-brief.md). For broad design work, maintain a [System Design Document](../artifacts-and-templates/system-design-document.md) as the evolving proposal. The [System Design Process](../system-design-process/02-requirements-and-workload.md) adds workload estimation and quality-attribute scenarios.

Check that requirements are unambiguous, testable, prioritized, and consistent. Review them with stakeholders before using them to justify architecture or technology decisions.
