# AGENTS.md

## Purpose

This repository is an educational software architecture lab focused on an e-commerce application and an event-driven data platform. Code must remain small, executable, and easy to connect to the documentation.

## Project rules

- Write project documentation in English, except for domain terms explicitly chosen otherwise.
- Prefer minimal, explicit, traceable changes linked to a specification.
- Treat `docs/system-design/` as the learning guide for the Delivery Framework, System Design Process, architecture concepts, technology selection, artifacts, and case studies.
- Use `docs/system-design/engineering-workflow/` for team workflow and pull request guidance, including the author template and reviewer checklist.
- Use `.github/pull_request_template.md` as GitHub's active pull request template and keep it aligned with its documentation under `docs/system-design/engineering-workflow/`.
- Maintain `CHANGELOG.md` for notable user-visible, architectural, or operational changes; keep entries concise and do not duplicate the full commit history.
- Keep the Delivery Framework's stages recognizable and ordered: requirements, core entities, API/protocols/endpoints, data flow, high-level design, and deep dives. Link methods and artifacts from the relevant stage instead of turning every method into a required step.
- Keep the System Design Process complementary: it frames the stakeholder problem, workload and quality goals, options, validation, and presentation of the design.
- Distinguish specifications and methods from tools. For example, OpenAPI is an API description specification and Swagger is a tooling ecosystem; C4 is a diagramming model; ADR records a decision; RFC solicits review; `EXPLAIN` inspects database query plans.
- Do not state technology adoption rankings, performance claims, or numerical targets without a dated, relevant source or project-specific measurement. Identify survey scope and uncertainty.
- Use examples as illustrative scenarios, not claims about the internal designs of named companies. Keep recommendations conditional on requirements and trade-offs.
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
  system-design/               Learning guide, processes, concepts, technology, artifacts, and case studies
  superpowers/specs/           Approved designs and specifications
```

The paths above describe the documentation currently present in the repository. Add code, tests, ADRs, or task-planning directories when the corresponding project work introduces them; do not treat absent directories as existing structure.

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
- for documentation changes, verify relative Markdown links and run `git diff --check`.

## System design documentation conventions

- Keep documentation in English and use the existing names and structure under `docs/system-design/`.
- Explain when and why a pattern or artifact is useful, its trade-offs, and how it connects to a Delivery Framework or System Design Process stage.
- Use measurable examples only when their assumptions and source are clear. Mark unknown targets as questions or assumptions rather than inventing commitments.
- Place reusable forms in `docs/system-design/artifacts-and-templates/`; place worked, explicitly illustrative scenarios in `docs/system-design/case-studies/`.
- Update the relevant index and `README.md` links when adding a documentation page. Keep relative links valid.
- For diagrams, preserve an editable source when practical and choose the notation for its audience and question (for example C4 for system boundaries, sequence diagrams for interactions, BPMN for business processes, and ER diagrams for relational structure).

## Boundaries

- **Always:** validate inputs, preserve invariants, document trade-offs, and run focused checks.
- **Ask first:** database changes, new dependencies, CI/CD changes, remote publication, deployments, or permission changes.
- **Never:** commit secrets, copy personal configuration, implement custom cryptography, or add distributed complexity only for demonstration.
