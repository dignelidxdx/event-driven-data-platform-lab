# Git Workflows

Branching models define how changes move from development to an integrated and releasable branch. Select a workflow based on release cadence, risk controls, and deployment capabilities; no model removes the need for automated checks and review appropriate to the change.

## Trunk Based Development

Developers integrate small changes frequently into a shared trunk, typically `main`, using short-lived branches or direct integration where policy allows. Feature flags and backward-compatible changes can separate deployment from feature exposure.

**Useful when:** CI is reliable, integration happens frequently, and the team can keep trunk releasable.

**Trade-offs:** demands fast feedback, disciplined small changes, and careful flag cleanup. Long-lived branches recreate integration cost.

## Gitflow

Uses long-lived branches such as `main` and `develop`, plus feature, release, and hotfix branches. It can make explicit release stabilization and support branches visible.

**Useful when:** a product has scheduled releases, several supported versions, or a formal stabilization period.

**Trade-offs:** more branch coordination, merges, and divergence; can slow continuous integration and create duplicated fixes. Avoid adopting it by default for a continuously deployed product.

## GitHub Flow and GitLab Flow

These names refer to platform-associated workflow guidance, not one immutable branching standard. Common variants use short-lived branches and merge requests or pull requests into a protected default branch; GitLab Flow also describes environment- or release-oriented approaches. Document the variant the team actually follows.

## Choosing a workflow

Compare release frequency, rollback needs, hotfix/support policy, CI speed, feature-flag maturity, and regulatory controls. Define branch lifetime, merge strategy, required reviews, status checks, release tagging, and rollback. Prefer the least complex workflow that provides the controls the product needs.

## Example

For this educational modular monolith, short-lived branches targeting `main` with review and CI would be a simple starting point if the team can keep `main` healthy. A release branch may be justified if the project later supports a maintained version while development continues on the next release. Record the choice and the conditions that would prompt reconsideration.
