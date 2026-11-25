# GCP Landing Zone and Deployment Architecture

## Design objective
Create a small but well-governed GCP foundation suitable for an early-stage research organization, with clear separation of environments, sensitive data and platform operations. Avoid creating projects and network layers that have no current owner or operational purpose.

## Proposed resource hierarchy

```text
Organization
├── Folders
│   ├── Platform
│   │   ├── platform-admin
│   │   ├── security-logging
│   │   └── network-services
│   ├── Nonproduction
│   │   ├── research-dev
│   │   └── research-test
│   └── Production
│       ├── research-data-prod
│       ├── research-compute-prod
│       └── clinical-sensitive-prod (only if required)
```

This is a target structure, not a requirement to provision every project on day one. Begin with a minimum viable set and split projects when data sensitivity, IAM, billing, network or operational ownership requires separation.

## Project responsibilities

| Project | Responsibility |
|---|---|
| platform-admin | Terraform state, CI/CD identities, shared platform administration |
| security-logging | Centralized audit log sinks, security monitoring |
| network-services | Shared VPC, DNS, controlled egress and connectivity |
| research-dev | Developer integration and synthetic test data |
| research-test | Integration and acceptance testing |
| research-data-prod | Production storage, ingestion, BigQuery |
| research-compute-prod | Cloud Batch, GKE if required, scientific compute |
| clinical-sensitive-prod | Restricted clinical data services, only when justified |

## Network baseline
- Prefer private workloads and controlled ingress.
- Use Shared VPC only when there is a genuine need for centrally managed networking across projects.
- Use Private Google Access for private access to supported Google APIs.
- Use Cloud NAT for controlled outbound connectivity where needed.
- Use Private Service Connect where supported and justified.
- Define egress allowlists and monitor unexpected outbound traffic.
- Evaluate VPC Service Controls for sensitive services; begin with dry-run and test all service dependencies before enforcement.
- Keep development and production trust boundaries separate.

## Identity
- Use Google Cloud Identity or federated workforce identities.
- Map groups to roles; avoid direct individual IAM grants.
- Use dedicated service accounts per workload and environment.
- Use Workload Identity Federation for GitHub Actions.
- Avoid service account key files.
- Apply least privilege and time-bound privileged access where available.
- Separate human administrator identities from runtime identities.

## Encryption and secrets
- Use Google-managed encryption by default where sufficient.
- Evaluate Cloud KMS customer-managed encryption keys for data classes and contractual requirements that need key control.
- Define key administrators, key users, rotation and recovery procedures.
- Store API credentials and certificates in Secret Manager.
- Never place credentials in Git, Terraform variables committed to source or CI logs.

## Logging and monitoring
- Centralize Admin Activity and relevant Data Access audit logs.
- Establish log retention and access policies.
- Monitor ingestion latency, failed files, pipeline errors, storage growth, BigQuery usage and compute spend.
- Alert on policy violations, unusual access and failed backup/restore checks.

## Terraform state
- Store remote state in a dedicated, restricted Cloud Storage bucket with versioning and tightly controlled IAM.
- Enable state locking using the supported Terraform GCS backend behavior.
- Separate state by environment and platform domain.
- Never store secrets in state unless unavoidable; treat state as sensitive.
- Use plan review and protected deployment approvals.

## Environment promotion
Developer branch -> pull request -> format/validate/security checks -> plan artifact -> authorized approval -> apply to dev/test -> acceptance -> authorized production promotion.

## Decisions requiring discovery
- Organization and billing account ownership.
- Required GCP regions and data residency.
- Existing corporate identity provider.
- Network connectivity to laboratory systems.
- BAA and applicable Google Cloud service eligibility for regulated workloads.
- Whether research and clinical data require separate projects or separate organizations.
