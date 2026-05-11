# CVMA System Architecture

## Layered view

```
+----------------------------------------------------------------------+
|  Presentation / Tenant Portals  |  Auditor Views  |  Technician PWA  |
+----------------------------------------------------------------------+
|                      Maintenance-as-a-Service API                     |
|     (contract, SLA, pricing, ticket lifecycle, billing reconciler)    |
+----------------------------------------------------------------------+
|   Compliance-Conditioned Work Order Orchestrator (Policy Engine)      |
|   - compiles procedure DAGs from CAG + Compliance Envelope            |
|   - emits step-level evidence requirements                            |
+----------------------------------------------------------------------+
|   Federated Cross-Vertical RUL & Anomaly Service                      |
|   - federated learning across tenants and verticals                   |
|   - CAG-anchored common embedding space                               |
|   - DP-budget keyed to envelope                                       |
+----------------------------------------------------------------------+
|   Canonical Asset Graph (CAG)                                         |
|   - vertical-agnostic ontology                                        |
|   - Ingestion Mapper bridges CMMS/EAM/AAS/proprietary feeds           |
|   - RSI Substitution Graph attached                                   |
+----------------------------------------------------------------------+
|   Tamper-Evident Provenance Ledger                                    |
|   - content-addressed Merkle log                                      |
|   - per-framework evidence renderers                                  |
+----------------------------------------------------------------------+
|   Substrate: FedRAMP-aligned enclave, KMS, HSM, OIDC, OPA             |
+----------------------------------------------------------------------+
```

## Module boundaries (see `src/`)

- `ingestion_mapper/` — schema-mapping layer with retrieval-augmented LLM grounded by the CAG, confidence scoring, HITL reconciliation, provenance tagging
- `canonical_asset_graph/` — graph store + ontology + query API
- `compliance_policy_engine/` — framework registry, OPA-style policy compilation, procedure-DAG generator
- `federated_rul/` — vertical-aware federated learner, embedding alignment to CAG, DP accounting
- `evidence_ledger/` — append-only Merkle log, signing pipeline, per-framework renderers
- `rsi_substitution/` — substitution graph + automated evidence pack for cross-coalition / cross-commercial substitutions

## Data flow (work order example)

1. Sensor / inspection event arrives → mapped to CAG node by `ingestion_mapper`.
2. Federated RUL flags asset for intervention → emits candidate FailureMode.
3. Policy engine looks up the asset's ComplianceEnvelope, compiles a
   procedure DAG (steps may differ for the same physical task depending on
   FedRAMP vs HIPAA vs ITAR context).
4. Technician executes, each step produces signed EvidenceArtifacts that
   land on the provenance ledger.
5. Auditor view renders an evidence pack scoped to the relevant framework.

## Trust boundaries

| Boundary | Crossings |
| --- | --- |
| Tenant ↔ shared CAG    | CAG-anchored embeddings only; raw telemetry stays in tenant enclave |
| Tenant ↔ federated RUL | Gradient updates with DP noise; budget tied to envelope |
| CVMA ↔ Auditor         | Read-only signed evidence packs |
| CVMA ↔ Vertical CMMS   | Bidirectional via `ingestion_mapper`, all writes logged |
