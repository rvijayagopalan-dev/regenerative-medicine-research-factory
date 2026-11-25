# Implementation Roadmap and MVP Acceptance

## Delivery strategy
Deliver an integrated research data platform first. Use Parkinson's as a bounded pilot, while keeping the canonical model reusable for ALS and retinal disease programs.

## Phase 0 — Discovery and architecture (Weeks 1–4)
Deliverables:
- Source-system inventory and integration feasibility.
- Research data classification and consent flow.
- Canonical identifiers and minimum viable data model.
- GCP region, organization and identity decisions.
- Architecture decisions and initial threat model.
- Backlog, ownership and cost estimate.

Exit: approved architecture baseline and pilot scope.

## Phase 1 — GCP foundation (Weeks 5–8)
Deliverables:
- Minimum project hierarchy and billing controls.
- IAM groups and workload identities.
- Network and private access design.
- Cloud Storage zones and BigQuery datasets.
- Terraform remote state and CI/CD.
- Audit logging, monitoring and budgets.

Exit: repeatable, secured nonproduction environment and approved production plan.

## Phase 2 — Integrated data platform (Weeks 9–16)
Deliverables:
- LIMS and ELN connectors.
- Sequencing and microscopy file ingestion.
- Instrument metadata and manifest contract.
- Clinical data trust boundary.
- Pub/Sub, Cloud Run and processing pipelines.
- Bronze, Silver and Gold zones.
- Data quality, quarantine, catalog and lineage.

Exit: integrated Parkinson's pilot data product with documented source provenance.

## Phase 3 — Research digital thread (Months 5–7)
Deliverables:
- Research subject, biospecimen, iPSC line and clone registry.
- Differentiation and cell model records.
- Experiment, assay and measurement model.
- Cross-system identifier mapping.
- Disease-specific data products.

Exit: sample-to-experiment traceability demonstrated.

## Phase 4 — Computational research (Months 8–10)
Deliverables:
- Nextflow and Cloud Batch proof of concept.
- Omics pipeline with versioned references and containers.
- Imaging analysis pipeline.
- Assay analytics.
- Analysis run registry and reproducibility package.

Exit: repeatable computational workflow using approved research data.

## Phase 5 — Knowledge and AI (Months 11–12)
Deliverables:
- Biomedical ontology mapping.
- Knowledge graph proof of concept.
- Vertex AI integration where justified.
- Dataset eligibility for model training.
- Multimodal research analytics prototype.
- Human-reviewed research assistant prototype.

Exit: governed AI research proof of concept, not clinical decision support.

## MVP acceptance criteria
1. Approved source systems are integrated through documented connectors.
2. Raw artifacts retain source identifiers and checksums.
3. Invalid or unresolved data is quarantined with a reason.
4. Curated data has validated relationships and mandatory metadata.
5. Consent/purpose checks are enforced for restricted data.
6. Researcher can navigate from coded subject or cell line to approved experiment and dataset.
7. Every analytical output references input dataset versions and execution metadata.
8. Audit, monitoring, recovery and cost controls are demonstrated.
9. Parkinson's pilot data product is reviewed and accepted by scientific owner.

## Dependencies
- Source-system vendor/API access.
- Laboratory network and instrument export capability.
- Scientific data steward availability.
- Consent and ethics interpretation.
- GCP organization, billing and identity access.
- Approved regions and contractual requirements.
- Representative synthetic or appropriately approved pilot data.

## Delivery governance
Use weekly architecture and delivery review, fortnightly scientific product review, formal change control for data contracts and security review before production promotion.
