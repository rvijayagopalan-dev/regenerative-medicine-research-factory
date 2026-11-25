# Target-State Enterprise Architecture

## Logical layers

| Layer | Responsibilities | Candidate GCP services / technology |
|---|---|---|
| Research experience | Scientist workbench, disease workspaces, data explorer | Cloud Run, Identity-Aware Proxy where appropriate |
| Source integration | LIMS, ELN, instruments, sequencing, microscopy, clinical and public data | API adapters, secure file transfer, Pub/Sub |
| Ingestion and orchestration | Events, validation, scheduling, routing | Cloud Run, Pub/Sub, Eventarc, Workflows, Cloud Scheduler |
| Processing | Batch, stream, transformations and scientific workflows | Dataflow, Dataproc Serverless, Cloud Batch, Nextflow |
| Artifact storage | Raw and derived large scientific files | Cloud Storage |
| Analytical platform | Structured and curated datasets | BigQuery, BigLake; evaluate Iceberg where interoperability requires it |
| Metadata and governance | Catalog, lineage, data quality, policy | Knowledge Catalog capabilities, Dataplex lineage capabilities where available, policy services |
| Scientific knowledge | Ontologies, graph relationships, evidence | RDF store or graph database selected after a proof of concept |
| AI and analytics | Omics, imaging, functional assay and multimodal models | Python, PyTorch, Vertex AI |
| Cross-cutting foundation | Identity, network, encryption, audit, observability, FinOps | IAM, VPC, Cloud KMS, Audit Logs, Cloud Monitoring |

Service names and product capabilities should be revalidated at implementation time. Select services based on region availability, supported integrations, data residency, contractual terms and cost.

## End-to-end logical flow

```mermaid
flowchart TB
  subgraph Sources["Research source systems"]
    LIMS[LIMS]
    ELN[ELN]
    SEQ[Sequencing]
    IMG[Microscopy / Imaging]
    INST[Lab instruments]
    CLIN[Clinical systems]
    PUB[Public datasets]
  end
  subgraph Ingestion["Integration and ingestion"]
    API[API adapters / Cloud Run]
    FILE[Secure file ingestion]
    EVENT[Pub/Sub / Eventarc]
    VALID[Validation and metadata enrichment]
  end
  subgraph Data["Biomedical data platform"]
    RAW[(Cloud Storage - Raw)]
    STD[(Standardized zone)]
    BQ[(BigQuery / Lakehouse)]
    PROD[Governed research data products]
  end
  subgraph Intelligence["Research intelligence"]
    KNOW[Ontology / Knowledge graph]
    COMP[Bioinformatics and scientific workflows]
    AI[Vertex AI / ML analytics]
    PORTAL[Research workbench]
  end
  LIMS --> API
  ELN --> API
  SEQ --> FILE
  IMG --> FILE
  INST --> FILE
  CLIN --> API
  PUB --> FILE
  API --> EVENT
  FILE --> EVENT
  EVENT --> VALID
  VALID --> RAW
  RAW --> STD
  STD --> BQ
  BQ --> PROD
  PROD --> KNOW
  PROD --> COMP
  PROD --> AI
  KNOW --> PORTAL
  COMP --> PORTAL
  AI --> PORTAL
```

## Trust boundaries
1. Identity and direct identifiers: tightly restricted custodian-controlled environment.
2. Sensitive coded research: approved research access, purpose limitation and audit.
3. General research analytics: de-identified or otherwise approved datasets, with re-identification risk assessed.
4. Public/open data: license and attribution requirements still apply.
5. AI training and inference: separate eligibility checks for dataset purpose, consent, license and model use.

## Deployment patterns
- API integration services: Cloud Run, with private ingress/egress where required.
- Eventing: Pub/Sub topics and subscriptions partitioned by domain and environment.
- File ingestion: controlled transfer into quarantine, followed by validation and promotion.
- Large batch bioinformatics: Nextflow on Cloud Batch; use GKE only where scheduling, specialized runtime or long-lived service needs justify it.
- Analytics: BigQuery and Cloud Storage; avoid copying large binary artifacts into relational tables.

## Key design constraints
- Do not assume all LIMS/ELN vendors expose compatible APIs or CDC.
- Do not send identifiable clinical data to general analytics by default.
- Do not equate de-identification with anonymity, especially for genomic data.
- Do not treat a knowledge graph as the system of record for raw scientific files.
- Do not promote data until schema, integrity, metadata and permitted-use checks pass.
