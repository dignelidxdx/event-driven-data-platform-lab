# AWS Architecture by Responsibility

This is a learning map from system responsibilities to AWS service candidates. It is not a deployment plan or a recommendation to use every service. Select a service only after workload, security, operations, and cost requirements are known. AWS Well-Architected evaluates workloads across operational excellence, security, reliability, performance efficiency, cost optimization, and sustainability; use those lenses to review trade-offs ([AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/userguide/waf.html)).

| Responsibility category | Candidate AWS services | Role in this example | Questions before choosing |
|---|---|---|---|
| Compute | AWS Lambda; Amazon ECS with AWS Fargate; Amazon EC2 | Run ingestion API and asynchronous processing workers | Are execution duration, memory, concurrency, startup, workload profile, and runtime model a fit? Is container control needed? |
| Object storage | Amazon S3 | Optional durable storage for raw batch files, large payloads, or replay inputs | Are batches too large for direct API/message transport? What encryption, lifecycle, retention, and access policies apply? |
| Operational database | Amazon RDS; Amazon DynamoDB | Persist acceptance/idempotency state and processing status | Is relational transaction/query behavior needed, or key-based access and managed scale? What consistency and recovery guarantees are required? |
| Analytics and warehouse | Amazon Redshift; AWS Glue; Amazon Athena; external Snowflake | Transform, catalog, query, or store analytical data depending on the target architecture | Is the goal a warehouse, lake, catalog, or query layer? What freshness, format, governance, and cost model are needed? |
| API and edge | Amazon API Gateway; Application Load Balancer; Amazon CloudFront; AWS WAF | Expose and protect the API; distribute cacheable assets or suitable responses | Is an API gateway needed for the chosen compute? Is content cacheable? What authentication, quotas, payload limits, and regional entry are required? |
| Networking | Amazon VPC, subnets, route tables, security groups, VPC endpoints, Route 53 | Define network reachability, isolation, private access, and naming | Which components must be public/private? What egress and service connectivity are needed? Avoid adding network layers without a boundary requirement. |
| Integration and messaging | Amazon SQS; Amazon SNS; Amazon EventBridge; AWS Step Functions | Buffer work, fan out notifications, route events, or orchestrate a multi-step workflow | Is this a task queue, pub/sub notification, event bus, or explicit workflow? What are ordering, replay, duplicate, retry, and DLQ needs? |
| Identity and security | AWS IAM; AWS KMS; AWS Secrets Manager; Amazon Cognito where user identity is required | Grant service permissions, encrypt data, manage secrets, and optionally authenticate users | What is the trust boundary? Can each workload use a narrow role? What data classification and key lifecycle apply? |
| Observability and operations | Amazon CloudWatch; AWS X-Ray; OpenTelemetry-compatible instrumentation | Collect logs, metrics, traces, alarms, and operational evidence | Which SLI/SLOs are measured? Can batch/correlation identifiers be followed end-to-end without leaking sensitive data? |
| DevOps and IaC | AWS CDK; AWS CloudFormation; AWS CodeBuild/CodePipeline; GitHub Actions or GitLab CI/CD | Version infrastructure, run checks, and promote artifacts through environments | Who can deploy? How are credentials federated? What approvals, drift handling, rollback, and environment policies are required? |

## Candidate learning architecture

One possible serverless learning variant is API Gateway → Lambda ingestion → SQS queue → Lambda worker → RDS/DynamoDB operational state and S3/analytics destination, with IAM roles, KMS encryption, CloudWatch signals, and infrastructure defined as code. This is only a sketch: transaction boundaries, event publication, batch sizing, analytics target, and operational guarantees still require design.

A container-based variant could use an Application Load Balancer and ECS/Fargate services with SQS-backed workers. Compare it against the serverless path using actual runtime, scaling, network, deployment, and cost needs rather than assuming one is universally better.

## Mapping principle

Keep domain use cases behind ports. The local adapters can be replaced by AWS adapters in an infrastructure layer without importing AWS SDK types into domain logic. Use an ADR for selected services and document the exit/migration cost when a service creates meaningful coupling.
