# Case Study: URL Shortener

## Scenario

Design a service that accepts a long URL and returns a short link that redirects visitors. The actual scale, product features, and service objectives are unspecified, so begin by eliciting them rather than assuming a target.

## Questions and requirements

- Are links public, private, expiring, editable, or associated with user accounts?
- What are expected creation and redirect rates, peak ratios, retention, and geographic distribution?
- What redirect latency and availability are required?
- How should abuse, malware, privacy, deletion, and custom aliases be handled?

## Design path

1. Define functional behavior and quality goals in the requirements brief.
2. Identify concepts such as short-link identity, destination, owner, expiry, and creation policy.
3. Specify create and redirect contracts, validation, conflicts, and idempotency expectations.
4. Trace link creation and redirect data flows.
5. Compare key-generation, storage, caching, and deployment options against the workload.
6. Deep dive on hot keys, cache consistency, abuse controls, data retention, and failure behavior.

## Candidate trade-offs

Random identifiers reduce predictability but require collision handling; sequential identifiers simplify allocation but may reveal volume or invite enumeration. A cache may reduce read latency but adds invalidation and staleness questions. The right choice depends on threat model, traffic, and product requirements.

Use a C4 context/container diagram, an ADR for a consequential identifier or storage decision, and capacity estimates once workload assumptions are available. Validate redirect performance and abuse controls with representative tests.
