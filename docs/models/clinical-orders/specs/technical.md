# Clinical Orders Technical Specification

## Status and safety boundary

This technical design is conceptual and educational. It authorizes no code, real data, integrations, medical decisions, or production deployment. The [functional specification](functional.md) contains unresolved business questions; architecture choices remain provisional until those questions are answered.

## Candidate domain contexts

| Context | Candidate responsibility | Candidate events |
|---|---|---|
| Order Management | Create, query, cancel, and coordinate order lifecycle | `ClinicalOrderCreated`, `ClinicalOrderCancelled` |
| Validation | Evaluate synthetic order rules and return a decision | `ClinicalOrderValidated`, `ClinicalOrderRejected` |
| Laboratory Capacity | Reserve/release fictional capacity and report progress | `CapacityReserved`, `CapacityReservationFailed`, `WorkCompleted` |
| Notification | Deliver synthetic status notifications | `OrderStatusNotificationRequested` |

These are candidate boundaries for learning DDD context mapping, not a commitment to deploy separate services. Begin as modules if implementation is later approved; create network boundaries only when ownership or operational requirements justify them.

## Workflow and consistency

A candidate workflow is submit → validate → reserve capacity → perform synthetic work → notify. Each step needs a defined timeout, retry, duplicate, and terminal-failure behavior. State is eventually consistent across contexts; the user-facing status model must explain pending and failed states.

Use a Saga only if the workflow spans independently committed steps and the business defines meaningful compensations. A capacity reservation might be released if later work fails, but not every completed external action can be undone. Compensation is a new business action, not a database rollback.

## Candidate entities and invariants

Possible concepts include ClinicalOrder, OrderItem, ValidationDecision, CapacityReservation, WorkflowStep, and NotificationAttempt. Exact fields and rules require domain-owner clarification. Candidate invariants include one coherent order lifecycle, duplicate-safe step handling, explicit ownership of each state, and auditable transitions using synthetic identifiers.

## Workload and quality attributes

No request volume, DAU, active operator count, latency target, availability target, retention, or recovery objective has been specified. DAU may not be a useful primary measure for a system-to-system workflow. Before technology selection, determine:

- orders and workflow starts per second, peak bursts, and fan-out per order;
- number and duration of workflow steps and external dependency timeouts;
- concurrent workflows, notification volume, and status-query mix;
- allowed end-to-end completion time, pending-state duration, and failure visibility;
- data retention, audit expectations, synthetic identifier policy, and recovery needs.

Candidate operational metrics include workflow completion time, age by state, step failure/retry rate, compensation attempts/outcomes, duplicate suppression, and notification delivery outcome. No thresholds are set until the scenario is validated.

## Technology approach

Start with a modular application and local adapters if the exercise is approved. A durable workflow engine, message broker, database, or orchestration service should be considered only after workload, durability, operational ownership, and failure requirements are specified. Do not choose AWS or another provider from the Saga pattern alone.

## Security and data handling

Use fabricated data that cannot identify a real person. No real prescriptions, diagnoses, health records, or provider credentials are allowed in this learning scenario. Any future real-world health workflow would require a separate legal, privacy, safety, security, and domain review before design or implementation.

## Validation and open decisions

- Clarify order actors, authorization, lifecycle states, and rejection rules.
- Define which steps are reversible and who owns compensations.
- Specify event contracts, idempotency scope, ordering, and audit requirements.
- Establish representative workload and completion/failure objectives.
- Decide whether the exercise remains a design-only case study or is approved for synthetic implementation.
