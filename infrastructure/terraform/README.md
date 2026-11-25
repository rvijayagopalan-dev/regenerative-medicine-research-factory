# Terraform Implementation Starter

This directory is reserved for the GCP infrastructure implementation. The current repository contains architecture specifications and starter contracts; no GCP resources are deployed by these documents.

## Planned module boundaries
- organization and project bootstrap (run by authorized platform administrators);
- IAM groups, service accounts and workload identity federation;
- network foundation and private connectivity;
- Cloud Storage buckets and lifecycle controls;
- BigQuery datasets and access boundaries;
- Pub/Sub topics, subscriptions and dead-letter handling;
- Cloud Run ingestion services;
- Dataflow jobs and service identities;
- Cloud Logging sinks, monitoring and alert policies;
- budget and cost controls.

## Environment layout
Use separate root configurations or deployment stacks for dev, test and prod. Reuse versioned modules. Keep environment-specific values in reviewed configuration files and secrets in Secret Manager or the approved CI secret store.

## State
Configure the GCS backend in a separately bootstrapped, restricted project. Enable object versioning and tightly restrict state access. State files can contain sensitive values.

## CI/CD expectations
- terraform fmt -check
- terraform validate
- static security and policy checks
- plan output retained as a build artifact
- approval before apply to shared or production environments
- Workload Identity Federation for GitHub Actions
- no long-lived service account key JSON files

## Before first deployment
Confirm organization and billing ownership, region, required APIs, quota, IAM authority, networking, data classification, contractual eligibility and security architecture. Review the plan with the platform owner. Never apply a generated plan to production without explicit review.
