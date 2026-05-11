# Compliance — Framework Registry and Policy Compilation

CVMA treats compliance frameworks as **declarative inputs** to the work order
compiler, not as audit-time overlays.

## Frameworks supported (initial set)

### Federal / Defense
- **FedRAMP** (Low / Moderate / High) — cloud baseline
- **NIST SP 800-53 Rev. 5** — control catalog
- **NIST SP 800-171 Rev. 3** — CUI protection
- **CMMC 2.0** (Levels 1–3) — DIB
- **DFARS 252.204-7012** — CUI clauses
- **ITAR / EAR** — export control
- **STIG / SRG** — DISA configuration baselines

### Commercial / Sectoral
- **SOC 2** Type II
- **ISO/IEC 27001:2022**
- **HIPAA** (Privacy + Security Rule)
- **PCI DSS 4.0**
- **NERC CIP** (energy / OT)
- **FDA 21 CFR Part 11** + **Part 820 QSR** (medical device maintenance)
- **EU NIS2 Directive**
- **EU AI Act** (where models drive maintenance decisions)

## Framework registry shape

```yaml
# compliance/frameworks/cmmc-l2.yaml
id: cmmc-l2
version: "2.0"
controls:
  - id: MA.L2-3.7.1
    title: "Perform maintenance"
    requires_evidence:
      - signed_work_order
      - tool_inventory_snapshot
      - technician_qualification_proof
    applies_to_envelopes:
      - any(asset.cui = true)
  - id: MA.L2-3.7.5
    title: "Nonlocal maintenance"
    requires_evidence:
      - mfa_session_record
      - session_termination_attestation
    applies_to_envelopes:
      - any(maintenance.remote = true)
```

The policy engine compiles these into procedure-DAG fragments that get
spliced into the canonical maintenance task at orchestration time.

## Output: per-step evidence requirements

The compiler annotates every step of the executable procedure with the set of
EvidenceArtifacts it must produce. The ledger then knows exactly what to
expect and surface to the auditor.
