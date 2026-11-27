# System Design Learning Guide

A practical guide to defining requirements, designing a system, evaluating trade-offs, and communicating a solution. The Event-Driven Data Platform Lab is the running example.

## How the guides fit together

The Delivery Framework is the technical sequence for shaping a solution: requirements, core entities, APIs and protocols, data flow, high-level design, and deep dives.

The System Design Process guides the broader design activity: framing the problem, understanding workload and quality goals, comparing options, validating risks, and presenting the proposal to stakeholders. It complements the Delivery Framework and links to supporting methods and artifacts at the relevant stages.

Concept references explain the methods and technical ideas used in these stages. Technology references support language and framework choices. Artifacts and templates provide optional ways to record and communicate the work. Case studies apply the guides to example systems.

## Delivery Framework

1. [Requirements](delivery-framework/01-requirements.md) — functional and non-functional requirements.
2. [Core entities](delivery-framework/02-core-entities.md) — central domain concepts, relationships, and invariants.
3. [API, protocols, and endpoints](delivery-framework/03-api-protocols-and-endpoints.md) — interfaces, communication protocols, endpoint behavior, and contracts.
4. [Data flow](delivery-framework/04-data-flow.md) — how data moves through the system, including events and transformations.
5. [High-level design](delivery-framework/05-high-level-design.md) — components, responsibilities, boundaries, and interactions.
6. [Deep dives](delivery-framework/06-deep-dives.md) — detailed analysis of important data, scalability, consistency, reliability, security, and operational concerns.

## System Design Process

1. [Problem and scope](system-design-process/01-problem-and-scope.md)
2. [Requirements and workload](system-design-process/02-requirements-and-workload.md)
3. [Domain and data](system-design-process/03-domain-and-data.md)
4. [Capacity and quality attributes](system-design-process/04-capacity-and-quality-attributes.md)
5. [Options and trade-offs](system-design-process/05-options-and-trade-offs.md)
6. [High-level architecture](system-design-process/06-high-level-architecture.md)
7. [Deep dives](system-design-process/07-deep-dives.md)
8. [Validation and risk review](system-design-process/08-validation-and-risk-review.md)
9. [Stakeholder walkthrough](system-design-process/09-stakeholder-walkthrough.md)

## Concepts

- [Architecture patterns](concepts/architecture-patterns.md)
- [Application patterns](concepts/application-patterns.md)
- [Design patterns](concepts/design-patterns.md)
- [Distributed systems](concepts/distributed-systems.md)
- [Design principles and anti-patterns](concepts/design-principles-and-anti-patterns.md)
- [Data and storage](concepts/data-and-storage.md)
- [Reliability and metrics](concepts/reliability-and-metrics.md)

## Technology selection and onboarding

- [Language comparison](technology/language-comparison.md)
- [Backend frameworks](technology/backend-frameworks.md)
- [Frontend frameworks](technology/frontend-frameworks.md)
- [Language onboarding guides](technology/onboarding/README.md)

## Engineering workflow

- [Engineering workflow guide](engineering-workflow/README.md)
- [Team ownership and CODEOWNERS](engineering-workflow/team-ownership.md)
- [Monorepo vs. multirepo](engineering-workflow/repository-strategy.md)
- [Trunk Based Development, Gitflow, and other Git workflows](engineering-workflow/git-workflows.md)
- [GitHub and GitLab](engineering-workflow/github-and-gitlab.md)
- [Pull request description template](engineering-workflow/pull-request-template.md)
- [GitHub pull request template](../../.github/pull_request_template.md)
- [Pull request review checklist, including AI-assisted changes](engineering-workflow/pull-request-review-checklist.md)

## Artifacts and templates

- [Artifacts and templates index](artifacts-and-templates/README.md)
- [Requirements brief](artifacts-and-templates/requirements-brief.md)
- [System Design Document](artifacts-and-templates/system-design-document.md)
- [Technology decision](artifacts-and-templates/technology-decision.md)
- [Architecture Decision Record](artifacts-and-templates/architecture-decision-record.md)
- [Request for Comments](artifacts-and-templates/request-for-comments.md)
- [C4 diagrams](artifacts-and-templates/c4-diagrams.md)
- [API and event contract](artifacts-and-templates/api-and-event-contract.md)
- [Capacity estimate](artifacts-and-templates/capacity-estimate.md)
- [Threat model](artifacts-and-templates/threat-model.md)
- [Reliability review](artifacts-and-templates/reliability-review.md)
- [Database query plan review](artifacts-and-templates/database-query-plan-review.md)
- [Runbook](artifacts-and-templates/runbook.md)
- [Design review checklist](artifacts-and-templates/design-review-checklist.md)

## Case studies

- [Case studies index](case-studies/README.md)
- [URL shortener](case-studies/url-shortener.md)
- [Social feed](case-studies/social-feed.md)
- [Messaging platform](case-studies/messaging-platform.md)
