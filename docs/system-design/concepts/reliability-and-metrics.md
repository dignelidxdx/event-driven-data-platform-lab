# Reliability and Metrics

Define what users experience, how the system will be observed, and how it should behave when dependencies fail. Metrics need a population, time window, and interpretation to be useful.

## SLI, SLO, and SLA

- **SLI:** a measured indicator of service behavior, such as the fraction of valid requests meeting a latency threshold.
- **SLO:** an internal objective for an SLI over a stated window.
- **SLA:** an external commitment with defined scope and consequences, when one exists.

Do not treat the terms as interchangeable. A target should specify eligible events, exclusions, aggregation window, and data source.

## Recovery and incident measures

- **RPO:** maximum acceptable data loss measured in time.
- **RTO:** target time to restore service after disruption.
- **MTTR:** mean time to restore or repair, with the chosen meaning stated explicitly because organizations use the acronym differently.
- **MTTD:** mean time to detect an incident. MTTI may mean mean time to identify or investigate in some teams; define it locally before using it.

Use workload and business impact to set targets. Pair metrics with alerting, runbooks, ownership, and recovery exercises; a dashboard alone does not establish operational readiness. See the [reliability review](../artifacts-and-templates/reliability-review.md) and [runbook](../artifacts-and-templates/runbook.md).
