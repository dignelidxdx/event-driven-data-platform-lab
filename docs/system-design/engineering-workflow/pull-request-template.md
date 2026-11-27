# Pull Request Description Template

GitHub uses the repository's [active pull request template](../../../.github/pull_request_template.md). Keep descriptions concise but give reviewers enough context to understand and verify the change.

In the active template, author instructions are HTML comments (`<!-- ... -->`), so GitHub hides them from the submitted PR description. The headings, checkboxes, and table remain visible for the author to fill in. The before/after screenshot section is optional and should be removed for changes without a visual impact.

The template asks the author to provide:

- **Description:** what changed.
- **Motivation:** why it is needed, linked to a requirement, issue, or decision.
- **Change type:** select one or more categories such as feature, bug fix, hotfix, documentation, refactor, maintenance, security, performance/reliability, breaking change, or other.
- **Scope and changes:** what is included and excluded.
- **Screenshots or recordings:** before/after evidence for user-visible changes; omit when not applicable.
- **Design notes:** decisions, alternatives, and relevant diagrams or records.
- **Verification:** tests, checks, manual verification, and any skipped checks with reasons.
- **Risk and rollout:** security, data, compatibility, migration, observability, and rollback considerations.
- **Reviewer guidance:** focus areas and specialist reviewers.
- **Author checklist:** requirement alignment, reviewable scope, edge cases, tests, module boundaries, security/data/performance, documentation/operations, and automated checks.

Treat checklist items as prompts for relevant evidence, not a demand to claim every concern applies to every change. Mark non-applicable items clearly. For documentation-only or mechanical changes, remove irrelevant screenshot or rollout details rather than adding noise.
