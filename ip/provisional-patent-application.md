# DRAFT — Provisional Patent Application

> **STATUS: DRAFT FOR ATTORNEY REVIEW.** This document is an engineering
> work product intended to be handed to a registered US patent practitioner
> for revision and filing on USPTO form **PTO/SB/16** (Provisional
> Application for Patent Cover Sheet). It is **not** legal advice and is not
> ready to file as-is. Inventor identification, fee selection (micro / small
> / large entity), oath/declaration, and signature blocks must be added at
> filing time.
>
> A provisional application is not examined and does not require claims
> (35 U.S.C. § 111(b); MPEP 601). To enjoy its priority date in a follow-on
> non-provisional, every concept later claimed must be **enabled** in this
> specification under 35 U.S.C. § 112(a). The specification below is
> therefore written for enablement breadth, not brevity.

---

## TITLE OF THE INVENTION

**Cross-Vertical Maintenance Systems Architecture with Compliance-Conditioned
Procedure Compilation, Failure-Mode-Anchored Federated Learning, and
Framework-Pluggable Evidence Rendering.**

## CROSS-REFERENCE TO RELATED APPLICATIONS

None at this time. To be completed by counsel.

## STATEMENT REGARDING FEDERALLY SPONSORED RESEARCH

None at this time. To be completed by counsel.

## TECHNICAL FIELD

The disclosure relates to computer-implemented systems for managing the
maintenance of physical, cyber-physical, and information-technology assets
across multiple industrial verticals under heterogeneous regulatory regimes.
More particularly, the disclosure relates to architectures that unify
vertical-specific maintenance data into a canonical asset graph; compile
executable maintenance procedures from canonical maintenance tasks subject to
asset-level compliance envelopes and signed substrate capability
attestations; train remaining-useful-life predictors via federated learning
across verticals; and render tamper-evident maintenance evidence into the
shapes required by a plurality of compliance frameworks.

## BACKGROUND

Computerized maintenance management systems (CMMSs), enterprise asset
management (EAM) platforms, digital-twin platforms (including those based on
the Asset Administration Shell standards), and federated-learning systems
for predictive maintenance are known. They tend to specialize in a single
vertical (manufacturing, fleet, IT infrastructure, healthcare, building
systems, or defense logistics) and treat compliance obligations (e.g.,
FedRAMP, CMMC, NIST SP 800-171, HIPAA, NERC CIP, ISO/IEC 27001, FDA 21 CFR
Part 11, NIS2, ITAR) as audit-time overlays rather than as runtime inputs to
the maintenance procedure itself.

Operators that span verticals — for example, defense primes that maintain
both classified and commercial estates; hospital systems that operate
medical equipment, fleet vehicles, and building systems; energy operators
that maintain OT, IT, and physical assets together — must run multiple
parallel maintenance stacks and reconcile audit pipelines manually. Existing
federated-learning approaches federate within a vertical or among parties
that hold different features of the same entity; they do not transfer
predictive maintenance knowledge across verticals at the granularity of
shared failure modes. Existing tamper-evident logs are general-purpose and
not specialized to project a single canonical maintenance event into the
evidence shapes that different frameworks require.

There is a need for an architecture that unifies these concerns.

## SUMMARY

A cross-vertical maintenance systems architecture (CVMA) is disclosed. The
architecture comprises a canonical asset graph (CAG); an ingestion mapper
that translates vertical-specific maintenance records into the CAG with
provenance and confidence scoring; a compliance-conditioned policy engine
that compiles canonical maintenance tasks into executable procedure DAGs as
a function of the asset's compliance envelope and a signed substrate
capability attestation, producing a signed compile trace; a federated
learning service that transfers remaining-useful-life knowledge across
verticals via failure-mode-anchored embedding alignment, with
differential-privacy parameters keyed to compliance envelopes; a
rationalization-standardization-interoperability (RSI) substitution
subsystem that derives ontology-aware part and procedure substitutions
gated by the compliance envelope and produces signed evidence packs; and an
evidence ledger that records signed maintenance events on a content-addressed
tamper-evident log and projects them into framework-specific evidence packs
via pluggable renderers.

The architecture's components share a single Compliance Envelope object that
propagates from the CAG into procedure compilation, federated learning
privacy accounting, substitution gating, and evidence rendering. This shared
propagation is a principal point of novelty.

## BRIEF DESCRIPTION OF THE DRAWINGS

The following drawings illustrate non-limiting embodiments. (To be prepared
in compliance with 37 CFR 1.84 and MPEP 1825 by a draftsperson.)

- **FIG. 1** — System block diagram of the CVMA showing presentation,
  maintenance-as-a-service API, compliance-conditioned orchestrator,
  federated RUL service, canonical asset graph, evidence ledger, and
  underlying substrate.
- **FIG. 2** — Data-flow diagram for a work order from telemetry ingestion
  through compile, execution, evidence collection, and per-framework
  rendering.
- **FIG. 3** — Schema diagram of the canonical asset graph including
  `Asset`, `Component`, `FailureMode`, `MaintenanceTask`,
  `EvidenceArtifact`, `ComplianceEnvelope`, `Tenant`, `Jurisdiction`,
  `Technician`, `Part`, and `Signal` node types and their edges.
- **FIG. 4** — Sequence diagram of the ingestion mapper: feed → profiler →
  candidate mapping → confidence scoring → optional HITL → provenance-tagged
  upsert.
- **FIG. 5** — Compilation diagram of the policy engine showing inputs
  (canonical task, compliance envelope, substrate capability attestation,
  tenant overlay) and outputs (executable procedure DAG with per-step
  evidence requirements, signed compile trace).
- **FIG. 6** — Comparative diagram showing the same canonical task
  `replace_disk_drive` compiling to five different procedure DAGs under
  SOC 2, HIPAA, FedRAMP-Moderate/CUI, ITAR, and NERC CIP envelopes.
- **FIG. 7** — Federated learning topology showing tenants across two or
  more verticals contributing gradients to a server that performs
  failure-mode-anchored embedding alignment with differential-privacy noise
  keyed to envelopes.
- **FIG. 8** — Substitution graph fragment with edges carrying
  `permitted_envelopes`, `failure_mode_coverage`, `qualification_basis`,
  `residual_risk_delta`, and `validity_window` annotations.
- **FIG. 9** — Evidence ledger architecture: append-only Merkle log,
  content-addressed artifact store, crosswalk store, renderer interface, and
  inclusion-proof endpoint.
- **FIG. 10** — Sequence diagram of compliance-aware substrate re-routing
  when a substrate capability attestation does not satisfy a required
  control.

## DETAILED DESCRIPTION

### 1. Overview

A CVMA system 100 (FIG. 1) comprises: a canonical asset graph store 110; an
ingestion mapper 120; a compliance-conditioned policy engine 130; a
federated learning service 140; an RSI substitution subsystem 150; an
evidence ledger 160; a maintenance-as-a-service (MaaS) API 170; and a
substrate 180 exposing a signed substrate capability attestation (SCA) 185.
Tenants 190 access the system through tenant portals 195. Auditors 198
access the system through auditor views 199.

### 2. Canonical asset graph (CAG)

The canonical asset graph store 110 holds nodes including, without
limitation: `Asset`, `Component`, `FailureMode`, `MaintenanceTask`,
`EvidenceArtifact`, `ComplianceEnvelope`, `Tenant`, `Jurisdiction`,
`Technician`, `Part`, and `Signal`. Edges include `HAS_COMPONENT`,
`SUBJECT_TO`, `MITIGATED_BY`, `PRODUCES`, `GOVERNED_BY`, `REQUIRES`, and
`SUBSTITUTABLE_FOR`.

The CAG schema is constructed to unify concepts from vertical-specific
standards including, without limitation, ISO 14224 (reliability and
maintenance data for petroleum, petrochemical, and natural gas industries),
IEC 81346 (reference designation systems), MIMOSA CCOM, ISA-95, IEC 62264,
Asset Administration Shell submodels (Industrie 4.0), the NIST CPS
Framework, HL7 FHIR Device, BACnet, Project Haystack, OpenTelemetry resource
semantics, MIL-STD-1388-2B LSAR, and others. By "unify" is meant that the
CAG schema admits node and edge encodings sufficient to represent the
maintenance-relevant subset of each named standard, and the ingestion
mapper 120 carries a translation policy from each standard's records into
CAG records.

Each `Asset` node carries, as a first-class attribute, a `ComplianceEnvelope`,
defined as a set of framework identifiers and per-framework severity
levels. The envelope is the central object that propagates into the policy
engine 130, the federated learning service 140, the substitution subsystem
150, and the evidence ledger 160.

The CAG store 110 may be implemented over a graph database (e.g., Neo4j,
JanusGraph), a relational database with graph extensions (e.g., Postgres
with Apache AGE), or a key-value store with adjacency indices. The choice
of backend is non-limiting.

### 3. Ingestion mapper

The ingestion mapper 120 receives records from heterogeneous
vertical-specific sources, including CMMS/EAM exports, AAS submodel
serializations, HL7 FHIR Device resources, BACnet trends, OpenTelemetry
signals, and MIL-STD-1388 LSAR dumps. For each source record the mapper
120 performs:

(i) **profiling** of source schema, including detection of field semantics
by signature matching against the CAG ontology;
(ii) **candidate mapping** derived by retrieval over CAG ontology nodes
augmented by a large-language-model (LLM) grounded in the retrieved nodes;
(iii) **confidence scoring** of each candidate mapping;
(iv) **routing to a human reviewer** for records whose confidence falls
below a configured threshold;
(v) **provenance tagging** of each committed record with a signed record
identifying the source system, source schema version, mapper model and
version, reviewer (if any), timestamp, and signature key identifier;
(vi) **upsert** of the canonical record into the CAG store 110.

The provenance record is itself an `EvidenceArtifact` and is anchored to
the evidence ledger 160.

### 4. Compliance-conditioned policy engine

The policy engine 130 receives:

(a) a canonical `MaintenanceTask` from the CAG;
(b) the `ComplianceEnvelope` of the `Asset` to which the task applies;
(c) a signed `SubstrateCapabilityAttestation` (SCA) 185 from the substrate
   180 on which the task will execute; the SCA declares, without
   limitation, FIPS mode and module certificate numbers, KMS/HSM identity,
   network egress class (open / restricted / air-gapped / one-way),
   identity providers, data residency, hardware root-of-trust class, and
   audit-log sink;
(d) optionally a tenant policy overlay further tightening control selection.

The policy engine 130 resolves each control in the envelope against a
control-to-evidence crosswalk (`compliance/frameworks.md`) and emits an
**executable procedure DAG** whose nodes are individual steps that either
(i) implement task-derived activities, (ii) implement
control-derived evidentiary activities, or (iii) compose both. Each step
carries the set of `EvidenceArtifact` kinds it must produce.

The policy engine 130 emits, alongside the executable DAG, a **signed
compile trace** containing: the identifiers and versions of the inputs (a)
through (d); the substrate's SCA digest; the crosswalk version; and the
emitted DAG's content hash. The compile trace binds the procedure to the
inputs and is itself an `EvidenceArtifact`.

When a control cannot be satisfied by the SCA (e.g., a step requires
FIPS-validated cryptography but the substrate reports `kms.fips=false`),
the policy engine 130 selects from a configurable response: (i) fail the
compile with a structured reason; (ii) emit a manual evidence step that
must be reviewed by a designated role; or (iii) **rewrite the DAG to
dispatch the unsatisfied steps to an alternative substrate** whose SCA
satisfies the control (FIG. 10), recording the rewrite and the chosen
alternative substrate's identity in the compile trace.

FIG. 6 illustrates how the canonical task `replace_disk_drive` compiles
under five different envelopes (SOC 2, HIPAA, FedRAMP-Moderate/CUI, ITAR,
NERC CIP), each producing a distinct procedure DAG with different
evidentiary steps (e.g., 800-88 attestation, BAA reference, dual-control
witnessed wipe, export-control screen, CIP-010 baseline diff).

### 5. Federated learning service for cross-vertical RUL

The federated learning service 140 trains remaining-useful-life (RUL) and
anomaly predictors over data held by a plurality of tenants 190 operating
assets in two or more distinct industrial verticals. The service 140
employs failure-mode-anchored embedding alignment in which:

(i) each tenant's local model produces, for each contribution, an
embedding indexed by `FailureMode` identifiers drawn from the CAG;
(ii) the server-side aggregator applies an **alignment loss** that reduces
embedding distance between contributions sharing a `FailureMode`
identifier across verticals while preserving vertical-specific nuisance
directions through a disentanglement penalty;
(iii) each gradient contribution is noised by a differential-privacy (DP)
mechanism whose parameters (e.g., ε, δ, clipping norm) are selected as a
function of the **strictest framework** in the `ComplianceEnvelope` of the
contributing asset;
(iv) the budget consumed by each contribution is recorded as a signed
`dp_budget_consumption_record` `EvidenceArtifact` on the evidence ledger
160; and
(v) the aggregated model is exposed for inference both within and across
verticals.

The differential-privacy mechanism may be Gaussian, Laplace, Rényi-DP-based,
or another suitable mechanism; the choice is non-limiting. The alignment
loss may be implemented as a contrastive loss, a centroid-attraction loss,
or another suitable formulation; the choice is non-limiting. The
disentanglement penalty may use orthogonality, adversarial discrimination,
or another suitable construction; the choice is non-limiting.

The service 140 may further expose **per-tenant DP budget accounts** that
are debited by recorded consumption and that are read by the MaaS billing
reconciler 175, such that DP budget is a billable resource on the service
contract.

### 6. RSI substitution subsystem

The RSI substitution subsystem 150 maintains a substitution graph attached
to the CAG. A substitution edge from a source `Part` to a target `Part`
carries a context tuple comprising at least: a `failure_mode_coverage` set;
a `qualification_basis` (standard citation or engineering memorandum
identifier); a `permitted_envelopes` set; a `residual_risk_delta` vector; a
`validity_window`; and a `signing_identity`.

A candidate substitution may be **derived at query time** by traversing
transitive standard-equivalence edges (e.g., STANAG ⇔ ISO ⇔ ASTM crosswalks
loaded from standards corpora including, without limitation, DLA ASSIST,
ISO, IEC, ASTM, and SAE), combined with ontology-derived
`failure_mode_coverage` from the CAG, and tenant-private engineering
approvals.

A derived candidate is **legal** with respect to an asset iff the asset's
`ComplianceEnvelope` is in the candidate's `permitted_envelopes`, the
candidate covers all `FailureMode` nodes mitigated by the original
`MaintenanceTask`, the `validity_window` covers the present time, and the
signature chain resolves to a trusted RSI authority for the envelope.

Upon application of a substitution, the subsystem 150 constructs a signed
**evidence pack** including the derivation trace, standard citations,
residual-risk vector, and engineering approval signatures, and anchors it
to the evidence ledger 160.

### 7. Evidence ledger

The evidence ledger 160 is an append-only, content-addressed log of
`EvidenceArtifact`s. Each artifact is identified by a content identifier
(e.g., sha-256 of a canonical serialization). The ledger comprises a
tenant-partitioned Merkle structure exposing inclusion proofs to clients.

A **crosswalk store** maps canonical evidence-kind identifiers to control
identifiers in each supported framework. A **renderer interface** admits
framework-specific renderer modules; each renderer projects the log into
the package shape expected by its framework (for example, FedRAMP SSP
appendices, CMMC SPRS evidence, HIPAA log exports, NERC CIP MOD-026
packages, ISO/IEC 27001 audit binders, EU NIS2 reporting bundles, and
21 CFR Part 11 records).

The ledger 160 may optionally anchor periodic roots to a public
transparency log or to a permissioned consortium chain, providing
third-party point-in-time witness without exposing tenant data.

### 8. Maintenance-as-a-service (MaaS) API

The MaaS API 170 exposes service-contract lifecycle (`draft`, `quoted`,
`active`, `in-grace`, `terminated`, `renewed`), SLA tiers (response time,
MTTR target, proactive coverage), and pricing plans (per-asset, per-event,
per-RUL-hour-recovered, hybrid). The billing reconciler 175 reads the
evidence ledger 160, groups events by contract, applies the plan, and
emits invoices with cryptographic backing. DP budget consumption recorded
by service 140 is read here as a billable resource.

### 9. Substrate

The substrate 180 may comprise a managed Kubernetes plane, a FedRAMP
GovCloud enclave, a single-tenant high-baseline enclave, an air-gapped
on-premise installation, or a ruggedized edge appliance. The substrate
exposes the SCA 185 as a signed JSON document declaring its capabilities.
The same CVMA application logic is deployed across substrate profiles; the
profile differences are read into compilation as SCA inputs.

### 10. Process flow

In a representative process flow (FIG. 2):

(s1) telemetry or inspection events from a vertical-specific source arrive
at the ingestion mapper 120 and are written to the CAG 110 with
provenance;
(s2) the federated learning service 140 emits an RUL signal indicating an
impending failure mode on an asset;
(s3) the policy engine 130 retrieves the canonical maintenance task, the
asset's compliance envelope, and the current SCA 185, and compiles an
executable procedure DAG with a signed compile trace;
(s4) the technician executes the DAG; each step produces signed
`EvidenceArtifact`s which are written to the evidence ledger 160;
(s5) on request, the evidence ledger renders an evidence pack scoped to a
specified framework, returning it to an auditor view 199.

### 11. Variants and non-limiting elaborations

- The CAG may be partitioned per-tenant, per-region, or globally; the
  ingestion mapper provenance and the policy engine compile-trace make
  partitioning auditable.
- The LLM in the ingestion mapper may be on-tenant or shared; on-tenant
  deployment is preferred for envelopes restricting model export.
- The federated learning topology may be star (single aggregator),
  hierarchical (per-vertical aggregators feeding a meta-aggregator), or
  decentralized with secure aggregation; the alignment loss is independent
  of topology.
- The substitution subsystem may operate in advisory or enforcing mode; in
  enforcing mode, non-derivable substitutions are rejected at work-order
  compile time.
- The renderer set is extensible; new frameworks are supported by adding a
  renderer module and a crosswalk row.
- The compile trace, the ingestion provenance, the DP budget consumption
  record, and the substitution evidence pack are each `EvidenceArtifact`s,
  unifying the audit story for AI decisions, human decisions, and machine
  decisions on the same ledger.

### 12. Inventive distinctions over prior art

Although components are individually known — digital-twin platforms (e.g.,
US 2016/0247129 A1; US 2017/0286572 A1), federated learning systems (US
12,488,223 B2; US 12,033,074 B2), tamper-evident logs (e.g., RFC 9162
Certificate Transparency, Sigstore Rekor), and continuous-compliance
platforms — the combinations recited here are not taught or suggested in
the prior art. In particular:

- No prior art teaches a vertical-agnostic canonical asset graph populated
  by a provenance-tagged, retrieval-grounded ingestion mapper that carries
  `ComplianceEnvelope` as a first-class node attribute.
- No prior art teaches compiling a maintenance procedure DAG from a
  canonical maintenance task as a function of the compliance envelope **and**
  a signed substrate capability attestation, with a signed compile trace.
- No prior art teaches cross-vertical federated learning of RUL via
  failure-mode-anchored embedding alignment, with differential-privacy
  parameters selected as a function of the strictest framework in the
  envelope and budget consumption logged as audit evidence.
- No prior art teaches deriving part substitutions by traversing
  standard-equivalence and ontology-derived failure-mode-coverage relations
  gated by the compliance envelope and auto-emitting a signed evidence
  pack.
- No prior art teaches an evidence ledger in which canonical evidence-kind
  identifiers map through a multi-framework crosswalk to pluggable
  framework-specific renderers.

The propagation of a single `ComplianceEnvelope` object through all five
subsystems is itself a non-obvious architectural choice; an artisan of
ordinary skill, presented with the component prior art, would not arrive
at this propagation absent the present teaching.

## ENUMERATED EMBODIMENTS (provided as enablement scaffolding; not claims)

E1. The system of [1] wherein the canonical asset graph schema encodes
nodes corresponding to at least ISO 14224 failure-mode classes, IEC 81346
reference designations, MIMOSA CCOM asset model concepts, Asset
Administration Shell nameplate submodels, and MIL-STD-1388 LSAR task
hierarchies.

E2. The system of [1] wherein the ingestion mapper computes a confidence
score by combining an LLM-derived semantic similarity with an ontology-
derived structural similarity, and routes records below a configurable
threshold to a human reviewer.

E3. The system of [1] wherein the substrate capability attestation
includes at least: FIPS-mode boolean with module certificate numbers; KMS
or HSM identity; egress class; data residency; and hardware root-of-trust
class.

E4. The system of [1] wherein, responsive to a control being unsatisfied
by the substrate capability attestation, the policy engine rewrites the
procedure DAG to dispatch unsatisfied steps to an alternative substrate
whose attestation satisfies the control.

E5. The system of [1] wherein the federated learning service applies a
contrastive alignment loss to embeddings indexed by failure-mode
identifiers and a disentanglement penalty to vertical-specific directions.

E6. The system of [1] wherein the differential-privacy parameters are
selected from the strictest framework's required ε, δ, and clipping norm
configured in the compliance envelope of the contributing asset.

E7. The system of [1] wherein each consumed differential-privacy budget
amount is recorded as a signed evidence artifact on the evidence ledger and
read by a billing reconciler as a billable resource on a service contract.

E8. The system of [1] wherein the substitution subsystem derives candidate
substitutions at query time by traversing transitive standard-equivalence
edges and ontology-derived failure-mode-coverage relations.

E9. The system of [1] wherein a substitution is rejected at work-order
compile time when the asset's compliance envelope is not in the candidate's
permitted-envelopes set.

E10. The system of [1] wherein the evidence ledger maintains tenant-
partitioned Merkle structures and exposes inclusion proofs.

E11. The system of [1] wherein the evidence ledger admits a plurality of
renderer modules, each renderer projecting log entries through the
crosswalk into a framework-shaped evidence package.

E12. The system of [1] wherein periodic Merkle roots of the evidence
ledger are anchored to a public transparency log or a permissioned
consortium chain.

E13. The system of [1] wherein the maintenance-as-a-service API exposes
pricing plans including per-RUL-hour-recovered terms whose recovery
calculation reads counterfactual estimates from the federated learning
service and execution proof from the evidence ledger.

E14. A method comprising the operations recited in [1].

E15. A non-transitory computer-readable medium storing instructions that,
when executed by one or more processors, cause the one or more processors
to perform the method of [E14].

## DEFINITIONS

"Compliance envelope" — a set of framework identifiers and per-framework
severity levels associated with an asset.

"Substrate capability attestation" (SCA) — a signed declaration by a
substrate of its security and operational capabilities, including at least
cryptographic-module certification status, key-management identity, network
egress class, data residency, and hardware root-of-trust class.

"Failure-mode-anchored embedding" — a vector representation of an asset
state that is indexed by, or conditioned on, a failure-mode identifier
drawn from the canonical asset graph, enabling alignment of representations
sharing a failure mode across verticals.

"Procedure DAG" — a directed acyclic graph whose nodes are maintenance
steps and whose edges encode ordering and data dependencies.

"Compile trace" — a signed record binding a procedure DAG to the canonical
maintenance task, compliance envelope, substrate capability attestation,
and crosswalk versions from which it was compiled.

"Evidence pack" — a framework-shaped assembly of `EvidenceArtifact`s,
produced by a renderer module from entries on the evidence ledger.

"Verticals" — distinct industrial domains such as manufacturing, fleet,
information technology infrastructure, healthcare, building systems,
energy/utility OT, and defense logistics.

## CLOSING

This provisional disclosure is intended to provide a written description
adequate to support claims directed to (i) the integrated CVMA system as
recited above; (ii) the compliance-conditioned procedure compilation method
with SCA inputs and signed compile trace; (iii) the cross-vertical
federated-learning method with failure-mode-anchored embedding alignment
and envelope-keyed differential privacy; (iv) the ontology-derived RSI
substitution method with envelope gating and auto-emitted evidence pack;
(v) the canonical evidence ledger with framework-pluggable renderers; and
(vi) corresponding non-transitory computer-readable media. Subject-matter
selection for the non-provisional follow-on is at the discretion of
counsel.

---

## FILING CHECKLIST (counsel to confirm)

- [ ] Specification (this document, edited per counsel)
- [ ] Drawings FIG. 1 – FIG. 10 in compliance with 37 CFR 1.84
- [ ] PTO/SB/16 Cover Sheet (Provisional Application for Patent)
- [ ] Inventor names + residences
- [ ] Correspondence address / customer number
- [ ] Entity size selection (micro / small / large)
- [ ] Filing fee per the current USPTO fee schedule
- [ ] Application Data Sheet (ADS) if priority/benefit claims apply
- [ ] Statement re: federally-sponsored research if applicable
- [ ] EFS-Web / Patent Center submission
- [ ] Calendar 12-month conversion deadline to non-provisional

## REFERENCES

- 35 U.S.C. § 111(b) — Provisional applications.
- 35 U.S.C. § 112(a) — Written description / enablement / best mode.
- 37 C.F.R. § 1.51, § 1.53(c), § 1.84.
- MPEP § 601 — Content of provisional and nonprovisional applications.
- MPEP § 1825 — Drawings.
- USPTO, *Drafting a Provisional Application* (Jun. 2023).
- USPTO Form **PTO/SB/16** — Provisional Application for Patent Cover Sheet.
