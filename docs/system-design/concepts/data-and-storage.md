# Data and Storage

Choose data models and storage behavior from access patterns, consistency needs, retention, scale, and operational constraints. Keep the domain model separate from vendor-specific persistence details where that boundary helps the application.

## ACID transactions

Atomicity, consistency, isolation, and durability describe properties of database transactions. Understand the database's isolation level and transaction scope; ACID does not automatically cover a workflow spanning independent services.

## OLTP and OLAP

OLTP systems serve operational reads and writes; OLAP systems support analytical queries over larger histories or aggregates. ETL and ELT describe approaches for moving and transforming data. Select pipelines and stores according to freshness, governance, and query needs.

## Schema evolution and migrations

Use backward-compatible changes when producers and consumers deploy at different times. Expand-and-contract, backfills, migration ordering, and lock behavior should be considered for live data changes.

## Query plans

`EXPLAIN` or a database equivalent shows how a query may be executed; an analyze mode may execute it and report observed behavior. Use representative query parameters and data distributions, and understand the database-specific implications before running execution modes on production workloads.

See [database query plan review](../artifacts-and-templates/database-query-plan-review.md) and the Delivery Framework [deep dives](../delivery-framework/06-deep-dives.md).
