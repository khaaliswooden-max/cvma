# evidence_ledger/

Append-only, content-addressed Merkle log of EvidenceArtifacts. The single
source of truth for every audit-relevant fact.

## Properties

- **Append-only**: writes only via signed envelopes.
- **Content-addressed**: artifact CIDs are sha-256 over a canonical
  serialization; ledger entries reference CIDs, not blobs.
- **Tenant-partitioned**: each tenant's Merkle subtree is independent; the
  root publishes anchors to a customer-visible inclusion-proof endpoint.
- **Framework-renderable**: a `Renderer<F>` projects subsets of the log into
  the evidence package shape framework `F` expects (FedRAMP SSP appendices,
  HIPAA log exports, CMMC SPRS evidence, NERC CIP MOD-026 packages, etc.).

## Why renderable matters

Without a renderer abstraction, supporting N frameworks requires N separate
audit pipelines. With one canonical log + N renderers, the cost of adding a
framework is *one renderer plus a crosswalk row in `compliance/`*. The
renderer concept itself is part of claim 5.

## Public surface (planned)

```
class EvidenceLedger:
    def append(envelope: SignedEnvelope) -> CID: ...
    def inclusion_proof(cid: CID) -> Proof: ...
    def render(framework: FrameworkID, scope: ScopeQuery) -> EvidencePack: ...
```

## Anchoring

Optional public anchoring (e.g. to a public Sigstore / certificate-transparency
style log, or a permissioned consortium chain) gives third parties a
verifiable point-in-time witness without exposing tenant data.
