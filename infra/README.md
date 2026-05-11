# Infra — Deployment Topology

CVMA must run simultaneously in commercial cloud, FedRAMP-Moderate / High
enclaves, and on-prem edge for ITAR / NERC-CIP / classified workloads. The
infra design treats each as a **deployment profile** of the same code base.

## Deployment profiles

| Profile | Substrate | Compliance posture |
| --- | --- | --- |
| `commercial`     | Managed K8s (e.g. EKS/GKE/AKS) + managed Postgres + S3-class object store | SOC 2, ISO 27001 |
| `fedramp-mod`    | AWS GovCloud or Azure Gov + FIPS endpoints + dedicated KMS | FedRAMP Moderate / CMMC L2 |
| `fedramp-high`   | Single-tenant enclave, customer-managed HSM, no shared planes | FedRAMP High |
| `itar-onprem`    | Air-gapped install, hardware root of trust, sneakernet sync | ITAR / Export-controlled |
| `edge-nercip`    | Ruggedized appliance in OT DMZ, one-way data diode upstream | NERC CIP, IEC 62443 |

The same `compliance_policy_engine` artifacts compile against each profile;
the difference is which controls are *automatically satisfied by the
substrate* vs which must be *enforced by the application*.

## Substrate components (logical)

- **KMS / HSM** — per-tenant key hierarchies, FIPS 140-3 modules where required
- **Identity** — OIDC + WebAuthn for technicians; PIV/CAC for federal
- **Policy** — OPA / Rego compiled from `compliance/frameworks.yaml`
- **Observability** — OpenTelemetry; logs signed at source, anchored into the provenance ledger
- **Object store** — content-addressed (sha-256) for evidence artifacts
- **Graph store** — backs the Canonical Asset Graph; pluggable (Neo4j, JanusGraph, Postgres+AGE)
- **Federated learning fabric** — secure aggregation, DP noise injection, gradient signing

## Why the profile model matters for the patents

Claim 2 of the provisional (compliance-conditioned work order orchestration)
requires that the *same logical maintenance task* compile to *different
executable procedures* depending on the asset's deployment profile and
compliance envelope. The infra design is what makes that runtime difference
verifiable: each profile exposes capability attestations that the policy
engine reads as inputs.
