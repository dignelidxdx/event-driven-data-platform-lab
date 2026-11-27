# Business and Learning Models

This repository contains three separate business or learning models. They are not parts of one product and are not integrated with each other by default. Each model has its own functional and technical specifications.

## Specification workflow for each model

Use this sequence for a new or materially changed model:

1. Clarify purpose, motivation, stakeholders, scope, and business outcomes.
2. Describe actors, use cases, business rules, boundaries, and acceptance criteria in `specs/functional.md`.
3. Quantify workload and quality attributes where evidence exists; label estimates and unknowns explicitly.
4. Derive the domain model, interfaces, data flow, architecture, technology choices, risks, and validation plan in `specs/technical.md`.
5. Review the documents with stakeholders, resolve open questions, and keep later plans, tasks, tests, and implementation traceable to the approved specification.

The separation follows the Spec-Driven Development idea of defining what and why before planning how. It does not require a particular tool or impose one universal template. See the [System Design Process](../system-design/system-design-process/README.md) and [Delivery Framework](../system-design/delivery-framework/README.md) for the project's design workflow.

## Models

- [E-commerce](e-commerce/README.md) — educational commerce application.
- [Event-driven data platform](event-driven-data-platform/README.md) — independent synthetic batch-ingestion platform.
- [Clinical orders](clinical-orders/README.md) — independent design-only learning scenario.

## Common specification sections

Functional specifications should explain purpose/motivation, stakeholders, goals and non-goals, scope and boundaries, actors, use cases, business rules, acceptance criteria, dependencies, assumptions, and open questions.

Technical specifications should explain domain/core entities and invariants; workload (for example requests per second, peak/burst behavior, DAU/MAU only when user activity is meaningful); quality attributes and measurable targets; API/event/data contracts; data flow; architecture and alternatives; technology rationale; security/privacy; observability; capacity/cost; failure and recovery behavior; testing/rollout; risks and unresolved decisions.

Do not invent DAU, request rates, SLOs, or capacity numbers. Distinguish measured baselines, stakeholder targets, estimates, and unknowns. Choose metrics that represent the feature's domain and user outcomes.

## Method references

- [GitHub Spec Kit quickstart](https://github.github.com/spec-kit/quickstart) — describes specifying what to build before planning how, clarifying ambiguity, then planning, tasking, implementing, and checking convergence.
- [GitHub Spec Kit](https://github.github.com/spec-kit/) — overview of the spec → plan → tasks → implementation workflow.
