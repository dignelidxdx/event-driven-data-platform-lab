# Case Study: Social Feed

## Scenario

Design a social product feed where users publish posts and follow other accounts. This is a generic learning scenario, not a description of any particular company's system.

## Questions and requirements

- Is the feed chronological, ranked, or both?
- What are posting, reading, and follower-count distributions, including skew and peaks?
- How fresh must posts appear, and what pagination behavior is expected?
- What are privacy, moderation, deletion, and visibility rules?

## Design path

1. Define feed and publishing behaviors plus latency, freshness, and availability goals.
2. Model accounts, follows, posts, visibility, and feed entries with ownership and invariants.
3. Specify post, follow, and feed query contracts.
4. Trace publication through event handling and feed materialization or query-time assembly.
5. Compare fan-out-on-write, fan-out-on-read, and hybrid approaches for the observed follower and read distributions.
6. Deep dive on ranking, hot accounts, duplicate delivery, deletion propagation, storage growth, and rebuilds.

## Candidate trade-offs

Fan-out-on-write can make reads fast but increases write amplification and handling of very high-follower accounts. Fan-out-on-read avoids precomputing every recipient feed but can increase read work and tail latency. A hybrid approach adds policy and operational complexity.

Use workload estimates to expose the read/write and skew assumptions. Consider event-driven processing and CQRS only if their decoupling or read-model benefits justify projection lag, replay, and repair requirements. Record material decisions in ADRs and validate with realistic skewed load tests.
