# Biomedical Data Platform and Lakehouse Architecture

## Purpose
Integrate research data from LIMS, ELN, sequencing, microscopy, instruments, clinical systems and public repositories while preserving source artifacts, scientific context, lineage and permitted use.

## Data domains
1. Research subject and cohort.
2. Consent and research authorization.
3. Biospecimen and tissue.
4. iPSC line, clone and characterization.
5. Differentiation protocol and batch.
6. Cell model and disease model.
7. Experiment and protocol.
8. Functional assay and measurement.
9. Omics and molecular observations.
10. Imaging and image-derived features.
11. Clinical research observations.
12. Computational analysis and research findings.

## Lakehouse zones

| Zone | Purpose | Typical content |
|---|---|---|
| Bronze / raw | Preserve source evidence | Original instrument files, FASTQ, BAM/CRAM, VCF, images, exports |
| Silver / standardized | Validate and normalize | Parquet, normalized metadata, identifiers, units, QC |
| Gold / curated | Publish governed research data products | Linked cohort, cell model, experiment, assay and omics datasets |
| Platinum / AI-ready | Approved model-development datasets | Features, embeddings, curated training/evaluation splits |

Platinum is an optional extension, not a mandatory fourth medallion layer.

## Storage design
- Cloud Storage for large binary and scientific artifacts.
- BigQuery for structured metadata, analytical tables and curated data products.
- Parquet for interoperable columnar data.
- HDF5, Zarr, AnnData/H5AD and domain-specific formats where appropriate.
- OME-TIFF or OME-NGFF for microscopy, depending on instrument and workflow compatibility.
- Keep references, checksums and metadata in the catalog; avoid placing large raw artifacts in relational tables.

## Proposed bucket families
- rmri-raw-{environment}
- rmri-quarantine-{environment}
- rmri-standardized-{environment}
- rmri-curated-{environment}
- rmri-ai-datasets-{environment}
- rmri-model-artifacts-{environment}
- rmri-audit-archive-{environment}

Bucket names, regions, retention and access boundaries must be finalized through the data classification and regional architecture process.

## BigQuery logical datasets
- rmri_reference
- rmri_research_core
- rmri_lims_staging
- rmri_eln_staging
- rmri_omics
- rmri_imaging
- rmri_assay
- rmri_clinical_restricted
- rmri_research_products
- rmri_ai_features

Use domain-oriented datasets and tables. Do not create a dataset for every experiment.

## Data product contract
Each product must define:
- Product owner and scientific steward.
- Purpose and intended research use.
- Schema and version.
- Source systems and source identifiers.
- Data classification and access policy.
- Consent/purpose eligibility rules.
- Quality checks and known limitations.
- Refresh cadence and freshness expectations.
- Lineage and transformation version.
- Retention and deletion/withdrawal handling.
- Consumer interface and support contact.

## Data quality dimensions
- Completeness of mandatory metadata.
- Validity of schema and controlled vocabulary.
- Uniqueness and stability of identifiers.
- Referential integrity across sample, cell line, batch and experiment.
- Unit and range validity.
- File integrity and checksum.
- Temporal consistency.
- Provenance completeness.
- Consent and permitted-use eligibility.

## Promotion rules
Raw -> standardized requires integrity, format, schema and metadata checks. Standardized -> curated requires semantic harmonization, identifier resolution, quality review and permitted-use checks. Curated -> AI-ready requires documented model purpose, dataset eligibility, cohort and bias assessment, split strategy and approval.

## Lifecycle
Use lifecycle policies only after scientific retention, reproducibility, consent and legal obligations are established. Do not automatically delete source artifacts merely because a derived table exists.
