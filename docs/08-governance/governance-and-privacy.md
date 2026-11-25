# Research Data Governance, Privacy and Security

## Governance objectives
Protect human-derived research data while enabling scientifically legitimate and reproducible use. Controls must follow data classification, consent, purpose, institutional policy and applicable legal obligations.

## Data classes

| Class | Examples | Baseline handling |
|---|---|---|
| Direct identifiers | Name, MRN, contact information | Restricted identity/custodian zone |
| Coded subject-level data | Coded genomic, clinical and phenotype records | Approved research access and audit |
| Confidential research | Unpublished assays, protocols and findings | Project and role-based access |
| Internal operations | Non-sensitive platform metadata | Organization access |
| Public data | Public repositories and publications | License, attribution and provenance |

Genomic data can remain re-identifiable even when conventional identifiers are removed. Treat it as sensitive unless a documented assessment supports a different classification.

## Consent and permitted-use policy
Represent consent and authorization as versioned records with:
- subject and source references;
- consent document/version and effective dates;
- approved purpose(s) and disease scope;
- data types and sharing restrictions;
- institutional/geographic restrictions;
- withdrawal and expiry status;
- IRB/ethics approval references where applicable;
- downstream dataset and analysis impact references.

A dataset access decision should evaluate user identity, role, project, purpose, data class, consent/authorization state, institutional approval and environment.

## Policy decision flow
1. Researcher requests a dataset for a declared purpose.
2. Identity and project membership are verified.
3. Data classification and access policy are evaluated.
4. Consent and permitted-use eligibility are checked.
5. Data custodian or ethics approval is obtained where required.
6. Least-privilege access is granted for a defined duration.
7. Access and downstream use are logged.
8. Withdrawal or policy changes trigger impact review and access revocation where required.

## GCP control mapping
- IAM: group-based least privilege.
- Cloud KMS: key control where required.
- Secret Manager: credentials and certificates.
- Cloud Audit Logs: administrative and data-access audit.
- Sensitive Data Protection: discovery and inspection workflows.
- VPC Service Controls: supported service perimeter controls after compatibility validation.
- Security Command Center: security posture and threat findings.
- Private connectivity and controlled egress: reduce exposure.

## Regulatory applicability
Assess HIPAA and business associate requirements where the organization, data and activities fall within scope. Assess Common Rule, IRB requirements, NIH Genomic Data Sharing Policy, GDPR and state privacy laws where applicable. Applicability is fact-specific and must be confirmed by qualified privacy, legal and research compliance personnel.

Do not claim that use of a particular GCP service automatically makes a workflow compliant. Confirm the applicable Google Cloud contractual terms, covered services, configuration requirements and organizational controls.

## AI-specific governance
Before model training or inference:
- verify dataset purpose and consent eligibility;
- check repository licenses and usage restrictions;
- document training data provenance;
- separate training, validation and test cohorts appropriately;
- evaluate leakage, batch effects and cohort bias;
- record model version, parameters and evaluation;
- require scientific review before interpreting outputs as findings.

## Retention and withdrawal
Retention, deletion and downstream withdrawal behavior must be defined by data class, source obligations, consent, law and scientific integrity requirements. Preserve an auditable record of actions. Do not assume that deletion of a source record automatically removes every derived artifact or model influence; define and assess the required downstream response.
