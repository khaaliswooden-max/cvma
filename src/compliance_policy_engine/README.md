# compliance_policy_engine/

Compiles a canonical MaintenanceTask + asset's ComplianceEnvelope + substrate
SCA into an **executable procedure DAG** with per-step evidence requirements.

## Inputs

- `task: MaintenanceTask` (from CAG)
- `asset: Asset` with its `ComplianceEnvelope`
- `sca: SubstrateCapabilityAttestation` (from infra)
- `tenant_policy_overlay: PolicyDocument` (tenant-specific tightening)

## Outputs

```
class CompiledProcedure:
    steps: list[Step]                 # ordered DAG of executable steps
    evidence_requirements: dict[StepID, set[EvidenceKind]]
    framework_satisfaction_map: dict[ControlID, set[StepID]]
    compile_trace: SignedDocument     # explainability artifact
```

## Why compile, not interpret?

Interpretation at run time would let an auditor argue that the framework
mapping was decided after the fact. Compilation produces a signed
`compile_trace` that pins the decision to (a) the asset's envelope at
compile time, (b) the substrate's SCA, and (c) the framework registry
versions. This is what makes claim 2 stand up.

## The same task, three procedures

A trivial example: `replace_disk_drive` on a server.

| Envelope         | Extra steps emitted |
| --- | --- |
| SOC 2            | sanitize-on-decom, signed removal log |
| HIPAA            | + DOD-5220.22-M wipe before removal + BAA reference |
| FedRAMP-Mod / CUI| + dual-control witnessed wipe + 800-88 attestation |
| ITAR             | + export-control screen on inbound replacement + cleared-personnel attest |
| NERC CIP         | + CIP-010 baseline diff before/after + change ticket cross-link |

Same canonical task; five different DAGs; one signed compile trace each.
