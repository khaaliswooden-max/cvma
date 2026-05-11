# canonical_asset_graph/

Owns the Canonical Asset Graph (CAG): schema, store adapter, query API.

## Public surface (planned)

```
class CAG:
    def upsert_asset(asset: Asset, provenance: Provenance) -> AssetID: ...
    def get_asset(asset_id: AssetID) -> Asset: ...
    def query(cypher_like: str, params: dict) -> ResultSet: ...
    def envelope_of(asset_id: AssetID) -> ComplianceEnvelope: ...
    def failure_modes(asset_id: AssetID) -> list[FailureMode]: ...
    def required_evidence(task_id: TaskID, env: ComplianceEnvelope) -> list[EvidenceKind]: ...
```

## Notes

- Backed by a graph store (Neo4j / JanusGraph / Postgres+AGE) chosen per
  deployment profile.
- Every write carries a `Provenance` (signer, source feed, mapper confidence).
- Schema is versioned; older clients can query by schema version.
