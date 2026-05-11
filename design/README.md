# Design — Canonical Asset Graph & System Architecture

The `design/` pillar defines the **Canonical Asset Graph (CAG)**: the
vertical-agnostic schema that every other pillar consumes.

## Source standards unified by the CAG

| Standard | Vertical of origin | Concepts unified into CAG |
| --- | --- | --- |
| ISO 14224                  | Oil & gas / petrochemical reliability | Failure modes, taxonomy of equipment |
| IEC 81346                  | Industrial systems                    | Reference designation (function/location/product) |
| MIMOSA CCOM / OSA-EAI      | Cross-industry asset mgmt             | Asset model + event semantics |
| ISA-95 / B2MML             | Manufacturing                         | Equipment hierarchy, work definitions |
| IEC 62264                  | Manufacturing operations              | Operations events |
| AAS (IDTA submodels)       | Industrie 4.0                         | Digital twin submodels, nameplate |
| NIST CPS Framework         | Cross-domain CPS                      | Aspects/concerns, trustworthiness |
| FMC-19 / MIL-STD-1388-2B   | Defense logistics (LSA)               | LSAR, maintenance task hierarchy |
| HL7 FHIR Device            | Medical devices                       | Device identifiers, lifecycle |
| BACnet / Haystack          | Building systems                      | HVAC / facility points |
| OpenTelemetry semconv      | IT infrastructure                     | Resource attributes, signals |

## Core node types

- `Asset` — physical or logical item with lifecycle
- `Component` — replaceable sub-part
- `FailureMode` — ISO-14224-style failure semantics
- `MaintenanceTask` — procedure DAG (steps, tools, parts, qualifications)
- `EvidenceArtifact` — proof produced or required by a task step
- `ComplianceEnvelope` — set of frameworks/controls active for an asset
- `Tenant`, `Jurisdiction`, `Technician`, `Part`, `Signal`

## Core edges

- `Asset --HAS_COMPONENT--> Component`
- `Asset --SUBJECT_TO--> FailureMode`
- `FailureMode --MITIGATED_BY--> MaintenanceTask`
- `MaintenanceTask --PRODUCES--> EvidenceArtifact`
- `Asset --GOVERNED_BY--> ComplianceEnvelope`
- `ComplianceEnvelope --REQUIRES--> EvidenceArtifact`
- `Part --SUBSTITUTABLE_FOR--> Part` (RSI substitution graph, see `rsi/`)

## Why this design enables the patent claims

The CAG is the single substrate that lets the policy engine, the federated
learner, and the evidence ledger speak the same language across verticals. The
provisional claims build on this; see `ip/provisional-patent-application.md`.

See `architecture.md` for the system-level diagram and module boundaries.
