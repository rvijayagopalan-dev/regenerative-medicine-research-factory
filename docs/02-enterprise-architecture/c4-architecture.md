# C4 Architecture Views

These are logical views for architecture review. They are not deployment diagrams or evidence of deployed resources.

## C1 — System context

```mermaid
flowchart LR
  Scientist[Research scientist]
  PI[Principal investigator]
  Bio[Bioinformatician]
  Custodian[Data / consent custodian]
  Systems[Existing LIMS / ELN / instruments]
  Public[Public research repositories]
  RMRIP((Research Intelligence Platform))
  Scientist --> RMRIP
  PI --> RMRIP
  Bio --> RMRIP
  Custodian --> RMRIP
  Systems --> RMRIP
  Public --> RMRIP
```

## C2 — Container view

```mermaid
flowchart TB
  subgraph GCP["GCP organization and approved projects"]
    UI[Research portal - Cloud Run]
    API[Integration APIs - Cloud Run]
    BUS[Pub/Sub]
    ORCH[Workflows / Scheduler]
    RAW[(Cloud Storage raw)]
    CUR[(Cloud Storage standardized)]
    BQ[(BigQuery curated)]
    CAT[Catalog / lineage]
    BATCH[Cloud Batch + Nextflow]
    AI[Vertex AI - future horizon]
    SEC[IAM / KMS / Audit / Network controls]
  end
  Sources[Source systems] --> API
  Sources --> RAW
  API --> BUS
  BUS --> ORCH
  ORCH --> RAW
  RAW --> CUR
  CUR --> BQ
  BQ --> CAT
  BQ --> UI
  CUR --> BATCH
  BATCH --> BQ
  BQ --> AI
  SEC -.cross-cutting.-> UI
  SEC -.cross-cutting.-> API
  SEC -.cross-cutting.-> RAW
  SEC -.cross-cutting.-> BQ
```

## C3 — Component view: ingestion service

```mermaid
flowchart TB
  SRC[Source adapter]
  AUTH[Authentication and source authorization]
  MAN[Manifest parser]
  CHECK[Checksum and format validation]
  META[Metadata and identifier validation]
  POLICY[Consent / permitted-use decision]
  QUAR[(Quarantine bucket)]
  RAW[(Raw bucket)]
  EVENT[Pub/Sub event]
  AUDIT[Audit and metrics]
  SRC --> AUTH --> MAN --> CHECK --> META --> POLICY
  CHECK -->|Invalid| QUAR
  META -->|Invalid| QUAR
  POLICY -->|Not permitted / unresolved| QUAR
  POLICY -->|Approved| RAW --> EVENT
  AUTH --> AUDIT
  CHECK --> AUDIT
  META --> AUDIT
  POLICY --> AUDIT
```

## C4 — Code-level design direction

The ingestion service should be decomposed into independently testable modules:
- Source connector interface.
- Authentication provider.
- Manifest and schema validator.
- File integrity verifier.
- Identifier resolver.
- Consent/purpose policy client.
- Storage promotion service.
- Event publisher.
- Audit and telemetry adapter.

Use contract tests for source connectors and integration tests against non-production GCP projects. Store secrets in Secret Manager and use workload identities rather than long-lived service account keys.

## Sequence — file ingestion

```mermaid
sequenceDiagram
  participant Lab as Lab instrument
  participant Gateway as Secure transfer gateway
  participant GCS as Cloud Storage quarantine
  participant Event as Pub/Sub
  participant Worker as Ingestion worker
  participant Policy as Research policy service
  participant Raw as Cloud Storage raw
  participant BQ as BigQuery metadata
  Lab->>Gateway: Export artifact + manifest
  Gateway->>GCS: Upload object
  GCS->>Event: Object finalized event
  Event->>Worker: Trigger processing
  Worker->>Worker: Checksum, format, schema and metadata validation
  Worker->>Policy: Evaluate purpose and data access eligibility
  Policy-->>Worker: Decision + policy reference
  alt Approved
    Worker->>Raw: Promote / register immutable source artifact
    Worker->>BQ: Write artifact and lineage metadata
  else Rejected or unresolved
    Worker->>GCS: Keep quarantined; record reason
  end
```
