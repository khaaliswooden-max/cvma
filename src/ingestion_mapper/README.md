# ingestion_mapper/

Bridges arbitrary vertical-specific feeds (CMMS exports, EAM, AAS submodels,
HL7 FHIR Device resources, BACnet trends, OpenTelemetry signals, MIL-STD-1388
LSAR dumps) into the CAG.

## Pipeline

```
raw feed --> profiler --> candidate mapping --> confidence scorer
                                          \--> retrieval over CAG ontology
                                          \--> LLM proposal (RAG-grounded)
                                                |
                                                v
                                         HITL review (if low confidence)
                                                |
                                                v
                                         CAG upsert + Provenance
```

## Why this isn't "just an ETL"

- Mapping proposals are derived by **retrieval over the CAG ontology**, not
  generated free-form. The LLM's role is to bridge naming + units; the
  schema target is fixed.
- Every mapping carries a **confidence score**. Below threshold → HITL.
- **Provenance** records: source system, source schema version, mapper
  model+version, reviewer (if HITL), timestamp, signature.
- The provenance is itself an EvidenceArtifact, anchored to the provenance
  ledger. This is what makes the CAG defensible to regulators.

This pipeline implements step (i) of claim 1.
