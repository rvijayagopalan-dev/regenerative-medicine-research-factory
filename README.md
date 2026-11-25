# Regenerative Medicine Research Factory

An architecture and product blueprint for an integrated **Regenerative Medicine Research Intelligence Platform (RMRIP)** on Google Cloud Platform.

The platform connects wet-lab research, iPSC disease modeling, functional assays, omics, imaging, clinical research data and computational workflows. The initial delivery priority is an integrated research data platform; scientific knowledge and AI capabilities are designed as subsequent horizons.

## Initial research programs
- Parkinson's disease
- Amyotrophic lateral sclerosis (ALS)
- Selected retinal diseases

## Architecture priorities
1. Integrate existing LIMS, ELN, sequencing, microscopy, instrument, clinical and public research data sources.
2. Establish a governed biomedical Lakehouse on GCP.
3. Create a canonical research model and sample-to-experiment digital thread.
4. Preserve scientific provenance, data quality and reproducibility.
5. Enable future biomedical knowledge graph and AI/ML capabilities.

## Repository map
- [Architecture documentation](docs/README.md)
- [Target-state architecture](docs/02-enterprise-architecture/target-state-architecture.md)
- [GCP landing zone](docs/03-gcp-architecture/gcp-landing-zone.md)
- [Biomedical data architecture](docs/04-data-architecture/biomedical-data-architecture.md)
- [Canonical research model](docs/04-data-architecture/canonical-research-model.md)
- [Ingestion patterns](docs/04-data-architecture/ingestion-patterns.md)
- [Governance and privacy](docs/08-governance/governance-and-privacy.md)
- [Implementation roadmap](docs/09-delivery/implementation-roadmap.md)
- [Interactive demo](demo/regenerative-medicine-research-factory.html)

## Architecture status
This repository contains a proposed logical and implementation architecture. It is not a deployed GCP environment, validated clinical system, validated scientific pipeline or production-ready Terraform implementation. Service selection, regulatory applicability, source-system compatibility, security controls and costs require validation before production use.

## Design principle
Integrate with existing LIMS and ELN systems rather than replacing them in the first release. Preserve source-system identifiers and immutable source artifacts while creating a governed intelligence layer around them.
