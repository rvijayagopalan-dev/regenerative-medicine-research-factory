# Research Data Pipelines

## Initial pipeline families
1. LIMS API extraction and normalization.
2. ELN metadata and attachment ingestion.
3. Sequencing artifact registration and Nextflow execution.
4. Microscopy artifact registration and metadata extraction.
5. Instrument file ingestion.
6. Clinical research export and controlled coding workflow.
7. Public dataset import with license and attribution tracking.

## Pipeline contract
Every pipeline must define owner, purpose, source, schema version, data classification, permitted-use check, idempotency key, retry behavior, quarantine behavior, output dataset, lineage record, monitoring and runbook.

## Engineering standards
- Version workflow definitions in Git.
- Pin containers by immutable digest.
- Record reference genome and annotation versions for omics workflows.
- Store parameters and execution metadata.
- Separate raw, intermediate and final outputs.
- Validate representative inputs and expected outputs.
- Test on synthetic or approved data in nonproduction.
- Document scientific assumptions and known limitations.

## Nextflow / Cloud Batch proof of concept
Begin with a small, well-defined pipeline and non-sensitive test data. Confirm container registry, service account permissions, storage access, network egress, logging, cost and retry behavior before scaling.
