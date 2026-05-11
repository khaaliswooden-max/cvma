# RSI Substitution Graph — design notes

## Edge predicate

Every substitution edge is of the form:

```
(Part A) --SUBSTITUTABLE_FOR { context }--> (Part B)
```

where `context` is a tuple:

```
{
  failure_mode_coverage: Set<FailureMode>,
  qualification_basis:   StandardCitation | EngineeringMemoCID,
  permitted_envelopes:   Set<ComplianceEnvelopeID>,
  residual_risk_delta:   RiskVector,
  validity_window:       TimeInterval,
  signing_identity:      KeyID
}
```

A substitution is **legal** at runtime iff:

1. The asset's current envelope is in `permitted_envelopes`.
2. All FailureModes mitigated by the original task are in
   `failure_mode_coverage`.
3. `validity_window` covers now.
4. The signature chain resolves to a trusted RSI authority for the envelope.

## Why this is not a lookup table

Implementations could store every substitution explicitly, but that does not
scale across coalition + commercial + tenant edges. Instead the graph is
**queried with derivation**: a candidate edge can be synthesized from
- transitive standard equivalences (e.g. STANAG ⇔ ISO ⇔ ASTM crosswalk),
- ontology-derived failure-mode coverage from the CAG,
- and tenant-private engineering approvals.

The synthesized edge is presented to the technician with the full derivation
trace, the technician signs the application, and the trace + signatures land
on the provenance ledger as the evidence pack.
