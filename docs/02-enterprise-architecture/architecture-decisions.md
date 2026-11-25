# Architecture Decision Records

Status values: **Proposed** means a direction for validation, not a final production approval.

| ADR | Decision | Status | Rationale |
|---|---|---|---|
| ADR-001 | GCP is the primary cloud | Proposed | User-selected platform |
| ADR-002 | Biomedical Lakehouse is the data architecture | Proposed | Supports structured, semi-structured and large artifact workflows |
| ADR-003 | Cloud Storage holds raw scientific artifacts | Proposed | Scalable object storage and lifecycle management |
| ADR-004 | BigQuery is the primary analytical store | Proposed | Managed analytics and SQL access |
| ADR-005 | API, event and file-based integration coexist | Proposed | Source systems have different integration capabilities |
| ADR-006 | Canonical research model links subjects, specimens, cell lines and experiments | Proposed | Enables traceability and cross-domain analytics |
| ADR-007 | Nextflow on Cloud Batch is the initial omics workflow candidate | Proposed | Portable workflow definitions and batch compute |
| ADR-008 | Clinical data uses a restricted trust boundary | Proposed | Separates direct identifiers and coded research data |
| ADR-009 | AI is a later horizon, not a prerequisite for MVP | Proposed | Protects initial delivery focus and cost |
| ADR-010 | Terraform and GitHub Actions are the infrastructure delivery approach | Proposed | Repeatable and reviewable deployments |

## Required validation before approval
- Source system integration feasibility.
- GCP region and service availability.
- Cost and quota estimates.
- Clinical and genomic data classification.
- Consent and permitted-use rules.
- Data retention and backup requirements.
- Security and contractual requirements.
- Scientific stakeholder acceptance.
