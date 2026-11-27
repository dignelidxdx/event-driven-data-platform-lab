# Database Query Plan Review

Use this review for a specific query or workload concern. `EXPLAIN` output is database-specific; record the database and version and do not assume that similar plan terms have identical behavior across engines.

## Query and context

- Query or operation:
- Database and version:
- Tables, indexes, and approximate data distribution:
- Representative parameters:

## Plan evidence

Attach or summarize the relevant plan. State whether the plan is estimated or observed. Execution modes such as `EXPLAIN ANALYZE` may run the query and have side effects or cost; understand the database behavior and use an appropriate environment.

## Findings

Describe scans, joins, estimates, actual row counts when available, sort or spill behavior, and measured latency. Avoid conclusions from a plan without representative data and parameters.

## Proposed change and validation

Record index/query changes, expected trade-offs, before/after measurement, and rollback considerations.
