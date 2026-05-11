# Framework crosswalk

CVMA models each control as a tuple of `(evidence_kind, trigger_predicate)`.
This lets one canonical EvidenceArtifact satisfy controls in multiple
frameworks simultaneously — the auditor view renders the right subset.

## Crosswalk excerpt

| Canonical Evidence            | NIST 800-53     | CMMC L2          | HIPAA Sec.       | ISO 27001:2022 | NERC CIP |
| ---                           | ---             | ---              | ---              | ---            | --- |
| signed_work_order             | MA-2, MA-3      | MA.L2-3.7.1      | §164.310(a)(2)   | A.7.13         | CIP-007 R3 |
| tool_inventory_snapshot       | MA-3(1)         | MA.L2-3.7.2      | —                | A.7.10         | CIP-007 R4 |
| technician_qualification_proof| PS-3, MA-5      | PS.L2-3.9.1      | §164.308(a)(3)   | A.6.1          | CIP-004 |
| mfa_session_record            | IA-2(1)         | IA.L2-3.5.3      | §164.312(d)      | A.8.5          | CIP-005 R2 |
| firmware_supply_chain_attest. | SR-4, SR-11     | SR.L2 (3.11–.13) | —                | A.5.21         | CIP-013 |
| dp_budget_consumption_record  | (custom)        | (custom)         | §164.514         | A.8.11         | — |

The crosswalk is the input that makes "one event satisfies many frameworks"
economically real. It's also the input to the auditor view: pick a framework,
get only the columns that map.
