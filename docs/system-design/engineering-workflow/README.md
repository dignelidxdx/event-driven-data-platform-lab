# Engineering Workflow

This section describes how teams organize repositories, collaborate on changes, and review code. It complements the [Delivery Framework](../delivery-framework/README.md): the framework shapes the solution, while these practices help a team build and review it safely.

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
