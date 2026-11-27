# Pull Request Review Checklist

Use this checklist to decide whether a change is understandable, correct for its stated purpose, and safe to merge. Review the intent and behavior first; do not treat line count or stylistic preference as a substitute for reasoning.

## 1. Understand the change

- [ ] Is the problem, expected outcome, and scope clear from the PR description?
- [ ] Can I connect the implementation to requirements, contracts, or an approved design?
- [ ] Are assumptions, exclusions, and requested reviewer focus stated?
- [ ] Is the diff one coherent change? If its size prevents meaningful review, can it be split safely?

There is no universal safe line limit. Changed lines, number of files, generated code, migrations, and conceptual complexity all affect review effort. Treat a very large or broad diff as a prompt to ask for decomposition or a guided review, not as an automatic rejection rule.

## 2. Check behavior and correctness

- [ ] Does the code implement the intended behavior, including relevant boundary and failure cases?
- [ ] Are validation, authorization, invariants, errors, retries, and idempotency handled where required?
- [ ] Are concurrency, ordering, transaction, and consistency assumptions correct for this workflow?
- [ ] Are tests meaningful and aligned with observable behavior? Do they cover critical success and failure paths?
- [ ] Do automated checks pass, or are skipped checks and follow-up actions explicitly explained?

## 3. Check maintainability and architecture

- [ ] Are responsibilities cohesive and names understandable in the domain language?
- [ ] Does the change respect module boundaries and dependency direction?
- [ ] Are abstractions and patterns justified by an actual variation or boundary?
- [ ] Is duplicated or overly complex logic introduced without a clear reason?
- [ ] Are API, event, database, and configuration changes compatible or accompanied by a migration plan?

## 4. Check security, data, and operations

- [ ] Could the change expose secrets or personal data, weaken access control, or create injection/resource-exhaustion risks?
- [ ] Are dependencies, permissions, and trust boundaries changed intentionally?
- [ ] Are migrations, backfills, destructive actions, rollback, and data retention considered?
- [ ] Are logs, metrics, traces, alerts, and runbooks updated when operational behavior changes?
- [ ] Are performance claims supported by representative measurement when performance is material?

## 5. Review changes built with AI assistance

Apply the same quality bar as for any change, with explicit attention to unsupported assumptions and plausible-looking errors:

- [ ] Has the author traced the implementation and can explain the key decisions and control flow?
- [ ] Are APIs, library behavior, versions, and security claims verified against project configuration or authoritative documentation?
- [ ] Were generated tests inspected for meaningful assertions, not just execution or implementation-shaped expectations?
- [ ] Were generated code, dependency additions, licenses, configuration, and copied snippets reviewed?
- [ ] Were untrusted inputs, authorization, secret handling, concurrency, error paths, and resource limits checked explicitly?
- [ ] Is AI use disclosed when repository or organizational policy requires it? No disclosure is required by this template absent such a policy.

AI tools can help summarize or inspect a diff, but their output is a review aid, not approval evidence. A human reviewer remains responsible for the decision and should verify findings in the code and checks.

## 6. Give actionable feedback

- [ ] Does each blocking comment describe the concrete risk and a path to resolve it?
- [ ] Are preferences or optional polish clearly distinguished from correctness or safety blockers?
- [ ] Is the review outcome explicit: approve, request changes, or comment with no merge decision?

## References

- [GitHub: reviewing pull requests](https://docs.github.com/en/pull-requests/get-started/reviewing-pull-requests-quickstart)
- [GitHub: standardizing pull requests, code owners, and branch rules](https://docs.github.com/en/pull-requests/reference/managing-and-standardizing-pull-requests)
- [Google Engineering Practices: code review standard](https://google.github.io/eng-practices/review/reviewer/standard.html)
- [Google Engineering Practices: small changes](https://google.github.io/eng-practices/review/developer/small-cls.html)
