# GitHub and GitLab

GitHub and GitLab provide repository hosting and collaboration features. Both support pull or merge request review, automated pipelines, and controls for protected branches; exact names and capabilities depend on product edition and configuration.

## Pull and merge request flow

1. Open a PR/MR with purpose, scope, linked requirement, risks, and verification evidence.
2. Request reviewers using team ownership and code-owner rules where configured.
3. Review the diff against the [review checklist](pull-request-review-checklist.md); discuss design-level questions before focusing on line-level details.
4. Require relevant CI checks and approvals for protected branches.
5. Merge using the repository's documented strategy and preserve traceability to issues and decisions.

## Repository controls

Configure default-branch protection or rulesets to require appropriate status checks, reviews, and restrictions on force pushes or direct updates. Use least privilege for tokens and CI credentials. Keep branch policies aligned with the chosen [Git workflow](git-workflows.md); excessive mandatory checks can make small changes unnecessarily slow.

## CI and review automation

Use CI for reproducible formatting, linting, tests, compilation, dependency and security checks, and documentation validation as appropriate. Both platforms define pipelines as version-controlled configuration in the repository:

- **GitHub Actions** uses workflow files under `.github/workflows/`. Teams can start from workflow templates or reusable workflows for build, test, and deployment jobs.
- **GitLab CI/CD** commonly uses a `.gitlab-ci.yml` file and supports pipeline templates and reusable components/includes for build, test, and deployment jobs.

CI validates changes, usually on pushes and PRs/MRs. CD can mean continuous delivery, where a change is made ready for a controlled release, or continuous deployment, where it is deployed automatically after required checks. Define environments, approvals, secrets, permissions, rollback, and production gates before enabling deployment jobs. Do not place credentials directly in workflow files.

Make failures actionable and avoid treating green checks as proof of correct business behavior. Automated review tools, including AI-based tools, can surface candidate issues; a human must verify findings and make the review decision.

## Platform-neutral practices

Keep PR/MR descriptions in versioned templates where supported. Define ownership close to repository conventions, keep required checks documented, and periodically remove stale rules and workflows. Consult the platform's current documentation before relying on a particular feature or plan limitation.
