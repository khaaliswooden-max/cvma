# federated_rul/

Federated learning service for Remaining Useful Life (RUL) and anomaly
detection. Federates across tenants **and** across verticals using the CAG as
the common embedding space.

## Why cross-vertical works at all

Prior art federates across tenants of one vertical (e.g. a fleet of presses
across factories). CVMA exploits the fact that the CAG carries
**failure-mode-anchored embeddings** — two physically dissimilar assets that
share a FailureMode (e.g. bearing-spalling) can contribute gradients to that
mode's predictor regardless of whether one is in manufacturing and the other
in fleet.

The vertical-to-vertical transfer is mediated by an **alignment loss** that
pulls same-FailureMode embeddings together while keeping vertical-specific
nuisance directions disentangled.

## Privacy budget keyed to envelope

Each tenant carries a DP budget. Gradient contributions are noised so the
budget consumption matches the **strictest framework in the asset's
envelope**. Budget consumption is itself logged as an EvidenceArtifact
(`dp_budget_consumption_record`) — meaning DP is meterable and auditable.

## Public surface (planned)

```
class FederatedRUL:
    def register_tenant(...): ...
    def submit_gradient(tenant, asset_class, gradient, dp_budget_used): ...
    def aggregate_round(...): ...
    def predict_rul(asset_id) -> RULDistribution: ...
    def explain(asset_id) -> Explanation: ...   # for evidence pack
```

Implements claim 3.
