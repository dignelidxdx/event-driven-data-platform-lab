# Engineering Workflow

This section describes how teams organize repositories, collaborate on changes, and review code. It complements the [Delivery Framework](../delivery-framework/README.md): the framework shapes the solution, while these practices help a team build and review it safely.

Team workflow and engineering practices are related but distinct. Practices such as automated tests and continuous integration can support an architecture across different team processes. See [evolutionary architecture and engineering practices](../concepts/evolutionary-architecture.md) for guidance on checking important architectural qualities and boundaries as the system changes.

## Pull request workflow

1. GitHub automatically uses [`.github/pull_request_template.md`](../../../.github/pull_request_template.md) for new pull requests.
2. [Pull request description template](pull-request-template.md) is the documented, platform-neutral source for that template's structure.
3. [Pull request review checklist](pull-request-review-checklist.md) guides human review, including changes produced with AI assistance.

## Team and repository structure

- [Team ownership](team-ownership.md) — responsibilities, code ownership, and `CODEOWNERS`.
- [Repository strategy](repository-strategy.md) — monorepo and multirepo trade-offs.
- [Git workflows](git-workflows.md) — Trunk Based Development, Gitflow, and choosing a branching model.
- [GitHub and GitLab](github-and-gitlab.md) — PR/MR review, branch protections, and CI concepts.

Follow the repository's configured platform and policies; these guides explain concepts and trade-offs rather than requiring a particular vendor.

## Starting and tracking work

For a change, start with a concise **what, why, and how**: what outcome and scope are intended, why the problem matters, and how the proposed approach could address it. A useful reasoning path is **context → problem → options → recommendation**. It is a thinking aid, not another required artifact; reuse the relevant [System Design Process](../system-design-process/README.md) stages and expand the detail to fit the risk and audience.

At the beginning, identify dependencies, risks, required permissions or access, and external services or teams. Verify access and availability before implementation relies on them, so a missing prerequisite is visible early. Track the work's owner, status, blockers, and next step in the project's existing issue or task tracker. Keep that status current when the scope, dependencies, or decision changes; do not maintain a parallel task list in the RFC.

Use an [RFC](../artifacts-and-templates/request-for-comments.md) when a proposal needs feedback before commitment. It records the reasoning and review outcome; an [ADR](../artifacts-and-templates/architecture-decision-record.md) records a consequential decision after it is made. The existing [pull request description](pull-request-template.md) carries the what, why, how, and verification evidence into implementation review.
