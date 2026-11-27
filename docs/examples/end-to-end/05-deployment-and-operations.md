# Deployment and Operations Path

Production deployment is a later stage beyond the current local lab specification. This checklist describes the evidence and controls required before a real environment is called production-ready.

## Delivery pipeline

1. Run formatting, linting, unit/integration/contract tests, build, dependency checks, and infrastructure validation on each proposed change.
2. Package an immutable application artifact and deploy infrastructure from reviewed IaC.
3. Promote the same artifact through isolated environments; keep environment configuration and secrets out of source control.
4. Require explicit authorization and appropriate approval gates for production deployment.
5. Verify health, acceptance latency, processing lag, error rate, and DLQ activity after release; define rollback or forward-fix criteria.

GitHub Actions or GitLab CI/CD can run these workflows. Use short-lived/federated credentials where supported, least-privilege roles, protected environments, and auditable approvals. The exact pipeline is a project decision, not implied by using AWS.

## Infrastructure and environment readiness

- Separate development, staging, and production accounts/environments according to organizational controls.
- Define network boundaries, IAM roles, encryption, secret rotation, backups, retention, and audit logging.
- Specify queue retention, visibility timeout, retry limits, DLQ alarms, replay authorization, and duplicate-safe reprocessing.
- Define database migration and rollback/forward-fix strategy; rehearse backups and restore.
- Estimate service, data transfer, storage, logging, and idle costs; add budgets or alerts appropriate to the environment.

## Operational signals

Track request acceptance rate/latency, accepted-to-processed time, worker errors and retries, queue age/depth, DLQ depth/age, and analytical freshness. Dashboards and alarms should link to a runbook with safe diagnosis and recovery steps.

Set SLOs, RPO, and RTO with product and operations owners. Exercise dependency failure, backlog recovery, duplicate delivery, and restore procedures before claiming those objectives are met.
