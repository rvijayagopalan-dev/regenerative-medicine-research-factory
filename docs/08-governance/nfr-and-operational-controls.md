# Non-Functional Requirements and Operational Controls

Targets below are initial design objectives for an early-stage research platform. They are not measured performance, contractual SLAs or guarantees.

| Domain | Initial objective | Measurement |
|---|---|---|
| Data integrity | Checksum validation for all file artifacts | % artifacts with verified checksum |
| Provenance | All curated products trace to sources and transformations | lineage coverage |
| Metadata | ≥95% mandatory metadata completeness after pilot stabilization | completeness score |
| Ingestion | ≥99% successful processing excluding source outages and invalid input | accepted/eligible ingestion ratio |
| API availability | Target 99.9% for critical research APIs where supported | service uptime |
| Freshness | Source-specific SLA defined per connector | source-to-available latency |
| Security | No public access to sensitive research storage | policy and exposure scans |
| Reproducibility | Every released analytical product has execution metadata | reproducibility record coverage |
| Recovery | Initial target RPO 24h and RTO 8h for reconstructible platform services | restore exercises |
| Cost | Monthly budget and alerts per environment/workload | cost allocation and variance |

## Workload-specific recovery
- Source artifacts: retain source copies and validated backups according to retention policy.
- Consent and authorization records: define stricter recovery and change audit requirements.
- Curated data products: regenerate where possible from retained source and versioned pipelines.
- Temporary compute: design as disposable and restartable.
- Terraform state: versioned, restricted and recoverable.

RPO/RTO must be tightened where risk, institutional policy or regulation requires it.

## Observability
Collect:
- request latency and error rate;
- Pub/Sub backlog and oldest unacked message;
- ingestion throughput and rejection count;
- Dataflow job failures and worker utilization;
- Cloud Batch queue and task failures;
- BigQuery bytes processed and slot/compute usage;
- storage growth and lifecycle transitions;
- IAM and policy changes;
- security findings and unusual access.

Use correlation IDs across API, event, workflow and artifact metadata.

## Reliability patterns
- Idempotent processing.
- Bounded retries with exponential backoff.
- Dead-letter queues.
- Quarantine and replay with authorization.
- Backpressure and concurrency limits.
- Schema compatibility checks.
- Runbooks and alert ownership.
- Periodic backup restoration tests.

## Cost controls
- Budgets and alerts by project.
- Labels for program, environment, owner and workload.
- BigQuery partitioning and clustering based on query patterns.
- Query limits and review of expensive scans.
- Storage lifecycle policies after scientific retention approval.
- Autoscaled/batch compute for bursty workloads.
- GPU use only for justified tasks.
- Cost per ingestion source and per analysis workflow.

## Operational ownership
Every production service needs an owner, support route, deployment method, monitoring dashboard, alert thresholds, recovery procedure and data classification.
