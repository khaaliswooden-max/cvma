# src/ — module stubs

Each subdirectory is a placeholder for a runtime module. The contracts below
are stable enough for the patent claims to reference; implementations are not
yet present.

| Module | Responsibility | Patent claim it supports |
| --- | --- | --- |
| `canonical_asset_graph/`     | CAG schema, graph store adapter, query API | 1 |
| `ingestion_mapper/`          | Schema-mapping with RAG-grounded LLM, HITL reconciliation, provenance tagging | 1 |
| `compliance_policy_engine/`  | Framework registry → procedure-DAG compiler with SCA inputs | 2 |
| `federated_rul/`             | Vertical-aware federated learner over CAG-anchored embeddings | 3 |
| `rsi_substitution/`          | Substitution graph + auto-evidence pack | 4 |
| `evidence_ledger/`           | Content-addressed Merkle log, per-framework renderers | 5 |
