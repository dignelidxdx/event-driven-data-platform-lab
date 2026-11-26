# AGENTS.md

## Purpose

This repository is an educational software architecture lab focused on an e-commerce application and an event-driven data platform. Code must remain small, executable, and easy to connect to the documentation.

## Project rules

- Write project documentation in English, except for domain terms explicitly chosen otherwise.
- Prefer minimal, explicit, traceable changes linked to a specification.
- Keep the domain independent from frameworks, databases, and transports.
- Do not add microservices, brokers, or distributed infrastructure without a documented reason.
- Do not include credentials, absolute paths, personal information, or local configuration.
- Do not modify or remove tests to hide a failure.
- Record every significant architectural decision in `docs/adr/`.
- Link diagrams and documentation from `README.md`.

## Required feature workflow

Features involving more than one file or an architectural decision follow this workflow:

1. **Scope check:** identify independent capabilities and dependencies.
2. **Specify:** write or update the specification before coding.
3. **Plan:** define components, order, risks, and verification points.
4. **Tasks:** split the plan into small tasks with acceptance and verification criteria.
5. **Implement:** execute one task at a time, starting with domain tests.
6. **Verify:** run relevant checks and report evidence.
7. **Document:** update ADRs, diagrams, and living documentation.

Do not move directly from a vague idea to implementation. An approved specification is the source of truth and must be kept current when decisions change.

## Working structure

```text
docs/
  adr/                         Architectural decisions
  architecture/                Diagrams and technical documentation
  superpowers/specs/           Approved designs and specifications
tasks/
  plan.md                      Approved technical plan
  todo.md                      Implementable task list
src/                           Application code
tests/                         Unit, integration, and contract tests
```

## Implementation conventions

- Use strict TypeScript and English names for public code.
- Use domain-driven language for entities, commands, and events.
- Place interfaces at layer boundaries and keep adapters behind ports.
- Keep use cases small, invariants in the domain, and validation at the edges.
- Name events in the past tense, for example `OrderPlaced`.
- Each module must declare what it may and may not import.

## Minimum verification

Before considering a feature complete:

- run relevant domain and use-case tests;
- run integration or contract tests when a boundary changes;
- run lint, formatting, and compilation when available;
- check that no secrets or local paths were added;
- update specifications, tasks, and ADRs when a decision changed.

## Boundaries

- **Always:** validate inputs, preserve invariants, document trade-offs, and run focused checks.
- **Ask first:** database changes, new dependencies, CI/CD changes, remote publication, deployments, or permission changes.
- **Never:** commit secrets, copy personal configuration, implement custom cryptography, or add distributed complexity only for demonstration.
