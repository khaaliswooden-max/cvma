# rsi_substitution/

The RSI substitution service. See `rsi/substitution-graph.md` for design.

## Public surface (planned)

```
class RSI:
    def candidates(part_id, envelope) -> list[SubstitutionCandidate]: ...
    def derive(part_id, envelope, depth=2) -> list[DerivedSubstitution]: ...
    def evidence_pack(substitution_id) -> EvidencePack: ...
```

## What gets signed into the evidence pack

- Source standard citations (STANAG / MIL-STD / ISO numbers)
- Failure-mode coverage delta
- Residual-risk delta (vector)
- Engineering approval signatures (where required)
- Auditor-readable rationale

Implements claim 4.
