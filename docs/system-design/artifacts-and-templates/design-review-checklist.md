# Design Review Checklist

Use the questions that fit the change; a checked box is not a substitute for evidence.

## Problem and requirements

- [ ] Is the user or business problem and scope clear?
- [ ] Are functional behaviors testable and quality goals measurable or explicitly uncertain?
- [ ] Are workload assumptions and constraints visible?

## Domain and interfaces

- [ ] Are core terms, ownership, invariants, and data lifecycle clear?
- [ ] Are API and event contracts, errors, compatibility, and security expectations described?
- [ ] Can critical data flows and failure paths be traced?

## Architecture and trade-offs

- [ ] Do component responsibilities and dependency directions support the requirements?
- [ ] Are alternatives and material trade-offs recorded?
- [ ] Are diagrams at the right level for the audience and consistent with contracts?

## Risk and operations

- [ ] Have relevant capacity, consistency, threat, reliability, and recovery risks been reviewed?
- [ ] Is validation evidence or a plan defined for key assumptions?
- [ ] Are ownership, rollout, observability, and recovery needs addressed?

## Communication

- [ ] Are decisions, open questions, and required stakeholder alignment explicit?
