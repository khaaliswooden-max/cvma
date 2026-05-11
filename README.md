# CVMA — Cross-Vertical Maintenance Systems Architecture

CVMA is an architecture and reference implementation for a maintenance platform
that operates across industrial verticals (manufacturing, fleet, IT
infrastructure, medical devices, building systems, defense logistics) under a
single canonical asset model, with compliance posture treated as a first-class
input rather than an audit-time afterthought.

## Pillars

| Pillar | What it does |
| --- | --- |
| `design/`     | Canonical Asset Graph (CAG) + system architecture |
| `infra/`      | Deployment topology, FedRAMP-aligned enclave model |
| `rsi/`        | Rationalization / Standardization / Interoperability — cross-coalition and cross-commercial substitution graph |
| `compliance/` | Framework registry (FedRAMP, CMMC, NIST 800-171, HIPAA, ITAR, NERC-CIP, NIS2) and policy compilation |
| `maas/`       | Maintenance-as-a-Service contract, SLA, and pricing model |
| `ip/`         | Prior-art survey, patent ideation, provisional patent application |
| `src/`        | Module stubs: ingestion mapper, policy engine, federated RUL, evidence ledger |

## What makes CVMA different

Existing CMMS/EAM/digital-twin platforms specialize in one vertical and
retrofit compliance after the fact. CVMA inverts that:

1. **One canonical asset graph** spans verticals (ISO 14224, IEC 81346, MIMOSA
   CCOM, AAS submodels, NIST CPS framework concepts unified).
2. **Compliance envelope** propagates from asset → work order → evidence pack.
3. **Federated RUL** learns across tenants and across verticals using the
   canonical graph as the common embedding space.
4. **Tamper-evident provenance** renders per-framework evidence on demand.

See `ip/provisional-patent-application.md` for the formal description of the
claimed system.

## Status

Pre-alpha scaffold. No runtime yet — this is the architecture artifact,
prior-art survey, and provisional patent draft.
