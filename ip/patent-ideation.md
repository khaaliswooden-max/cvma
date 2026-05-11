# Patent Ideation — CVMA

This document enumerates candidate non-obvious patent concepts that emerged
from mapping the CVMA architecture against the prior-art survey
(`prior-art-survey.md`). Each candidate is framed as **problem → novelty →
claim sketch → prior-art gap → commercial leverage**.

The candidates are ranked by patent strength. The top concept (P-1) is the
**umbrella system claim**, and P-2 through P-5 are sub-system claims that can
stand alone, be filed as continuations, or be folded into the umbrella's
dependent claims. P-6 and P-7 are method-of-use claims that ride along.

---

## P-1 — Cross-Vertical Maintenance Systems Architecture (umbrella)

### Problem
Existing CMMS/EAM/digital-twin platforms are vertical-specific. Each retrofits
compliance as an audit overlay rather than treating it as a runtime input to
the maintenance procedure itself. Federated learning approaches federate
within a vertical. Tamper-evident logs exist but are not specialized to
maintenance evidence rendering. The combination of these limitations means
every operator running across verticals (e.g., a defense prime that also runs
commercial facilities; a hospital system that operates fleet vehicles) ends
up with N parallel maintenance stacks and N parallel audit pipelines.

### Novelty
A single architecture in which:
1. a **Canonical Asset Graph (CAG)** is populated by a **provenance-tagged ingestion mapper** spanning multiple vertical-specific schemas;
2. the CAG carries a **Compliance Envelope** attribute on each asset;
3. a **policy engine** compiles a canonical MaintenanceTask + Compliance Envelope + signed **Substrate Capability Attestation (SCA)** into an executable procedure DAG with per-step evidence requirements and a signed compile trace;
4. a **federated RUL service** transfers knowledge across verticals using FailureMode-anchored embedding alignment, with **DP budget keyed to the Compliance Envelope** and DP consumption itself logged as evidence;
5. an **evidence ledger** of signed maintenance events is rendered into per-framework evidence packs by pluggable renderers.

The Compliance Envelope is **the same object** read by (2), (3), (4), and (5).
This shared object is what binds the components into a single architecture.

### Claim sketch (independent)
> **A computer-implemented system for cross-vertical maintenance management,
> comprising:
> a canonical asset graph store holding asset nodes belonging to two or more
> verticals, each asset node carrying a compliance envelope attribute;
> an ingestion mapper configured to translate records from heterogeneous
> vertical-specific maintenance schemas into the canonical asset graph,
> assigning to each mapped record a confidence score and a provenance record
> signed by the mapper, wherein records below a confidence threshold are
> routed to a human reviewer prior to commit;
> a policy engine configured to receive (i) a canonical maintenance task,
> (ii) the compliance envelope of an asset to which the task applies, and
> (iii) a signed substrate capability attestation describing the execution
> environment, and to compile therefrom an executable procedure DAG having
> per-step evidence requirements, and to emit a signed compile trace pinning
> the compilation to the inputs;
> a federated learning service configured to receive gradient contributions
> from a plurality of tenants spanning two or more verticals, to align
> contributions in an embedding space anchored to failure modes of the
> canonical asset graph, and to enforce a differential-privacy budget for
> each contribution keyed to the compliance envelope of the contributing
> asset; and
> an evidence ledger configured to receive signed evidence artifacts produced
> by execution of the procedure DAG and to project the artifacts via
> framework-specific renderers into per-framework evidence packs.**

### Prior-art gap
No single reference in the surveyed corpus combines a vertical-agnostic
canonical asset graph with compliance-conditioned procedure compilation,
cross-vertical federated learning, and framework-pluggable evidence
rendering. Closest combinations cover at most two of these.

### Commercial leverage
The umbrella claim is the licensable surface. Any operator who moves to
multi-vertical maintenance with auditable AI and signed evidence packs would
practice within it.

---

## P-2 — Compliance-Conditioned Procedure-DAG Compilation

### Problem
Today, the maintenance task for replacing a part is the "same" regardless of
whether the asset holds CUI, PHI, or NERC-regulated configuration. The
differences live in PDFs and tribal knowledge. Auditors discover gaps
post-hoc.

### Novelty
A **compiler** that, at the moment the work order is generated, takes:
- a canonical maintenance task,
- the asset's compliance envelope,
- a signed SCA of the substrate that will host the work,

and emits an executable procedure DAG plus a **signed compile trace**
asserting which framework controls each step satisfies. Steps that cannot be
satisfied by the substrate either fail the compile, gain a manual evidence
step, or trigger a substrate re-route.

### Claim sketch (independent)
> **A method of generating an executable maintenance procedure, comprising:
> retrieving a canonical maintenance task and an asset compliance envelope
> from a canonical asset graph; receiving a signed substrate capability
> attestation; resolving each control identifier in the envelope against a
> control-to-evidence crosswalk; emitting an executable procedure DAG whose
> steps include both task-derived and control-derived steps, each annotated
> with required evidence kinds; producing a signed compile trace that binds
> the procedure DAG to the input task, envelope, attestation, and crosswalk
> versions.**

### Prior-art gap
Generic policy engines (OPA) and continuous-compliance platforms (Vanta,
Drata) do not compile *physical* maintenance procedures from controls; they
audit IT estates. OSCAL formalizes controls but stops short of procedure
emission. No surveyed patent ties substrate attestations into the
compilation.

### Commercial leverage
Independent of the umbrella; a stand-alone product for regulated industries.

---

## P-3 — Cross-Vertical Federated RUL via Failure-Mode-Anchored Embeddings, with Envelope-Keyed DP

### Problem
Federated learning for predictive maintenance today either (a) federates
across tenants of one vertical, or (b) does same-entity vertical FL where
two parties hold different features of the same entity (US 12,033,074 B2).
Cross-vertical knowledge transfer — e.g., bearing-spalling signatures
learned on industrial presses informing fleet wheel-hub bearings — is not
addressed. Privacy budgets are treated as flat per-tenant, not tied to the
strictest framework the asset participates in.

### Novelty
- Use the CAG's FailureMode nodes as **anchors** for a shared embedding space.
- Train with an **alignment loss** that pulls same-FailureMode embeddings together while disentangling vertical-specific nuisance.
- Gate each gradient contribution by the **strictest-framework DP budget** in the contributing asset's compliance envelope; log the budget consumption as a signed EvidenceArtifact.

### Claim sketch (independent)
> **A method of federated training of a remaining-useful-life predictor,
> comprising: receiving, from a plurality of tenants operating assets in two
> or more industrial verticals, gradient contributions associated with
> failure-mode identifiers from a canonical asset graph; computing an
> alignment loss term that reduces embedding distance between contributions
> sharing a failure-mode identifier across verticals while preserving
> vertical-specific directions; applying differential-privacy noise to each
> contribution with parameters selected as a function of the strictest
> compliance framework in the compliance envelope of the contributing asset;
> recording, for each contribution, a signed budget-consumption record on a
> tamper-evident ledger; aggregating noised contributions into a global model
> usable across verticals.**

### Prior-art gap
US 12,033,074 B2 covers vertical FL between *parties* about the *same
entities*; not cross-vertical FL across *different entities* unified by
ontology. No surveyed reference ties DP parameter selection to a compliance
envelope nor logs DP consumption as an audit artifact.

### Commercial leverage
Quantifiable privacy = quantifiable risk = price-able. The DP-as-meter
sub-concept underpins the MaaS pricing line.

---

## P-4 — Ontology-Derived RSI Substitution with Envelope Gating and Auto-Evidence Pack

### Problem
RSI substitution tables exist in defense logistics and OEM catalogs as
lookup tables. They do not encode whether a substitution is *legal under the
compliance envelope the asset currently lives in*, nor do they auto-produce
the evidence pack an auditor or program manager will demand.

### Novelty
Substitution is **derived** by:
- transitive standard equivalences (STANAG ⇔ ISO ⇔ ASTM crosswalks),
- ontology-derived failure-mode coverage from the CAG,
- and tenant-private engineering approvals;

then **filtered** by the asset's compliance envelope; then **packaged** with a
signed evidence pack carrying the derivation trace, citations, and
residual-risk vector.

### Claim sketch (independent)
> **A method of proposing a part substitution, comprising: receiving an asset
> identifier and an original part identifier; retrieving the asset's
> compliance envelope from a canonical asset graph; deriving candidate
> substitutions by traversing standard-equivalence edges and ontology-derived
> failure-mode-coverage relationships; filtering candidates to those whose
> permitted-envelopes set includes the asset's compliance envelope; for each
> remaining candidate, computing a residual-risk vector and constructing a
> signed evidence pack including derivation trace, standard citations, and
> the residual-risk vector; recording the evidence pack on a tamper-evident
> ledger upon application of the substitution.**

### Prior-art gap
No surveyed patent derives substitutions by ontology reasoning gated by
compliance envelope and emits a signed audit pack.

### Commercial leverage
Direct fit to defense logistics (NSN substitutions across coalition forces)
and to highly regulated commercial fleets (aviation, rail, medical device
service).

---

## P-5 — Canonical Evidence Ledger with Framework-Pluggable Renderers

### Problem
Each compliance framework expects its own evidence package shape (FedRAMP SSP
appendices, CMMC SPRS, HIPAA logs, NERC CIP MOD-026, 21 CFR Part 11). Today,
each is a separate pipeline. Cost scales linearly with frameworks.

### Novelty
A canonical log of signed maintenance events plus a **renderer protocol**
that projects subsets of the log into framework-specific evidence packs. A
single canonical event satisfies many frameworks via the crosswalk in
`compliance/frameworks.md`.

### Claim sketch (independent)
> **A computer-implemented evidence-management system, comprising: an
> append-only content-addressed log of signed maintenance-evidence
> artifacts, each artifact bearing a canonical evidence-kind identifier; a
> crosswalk store mapping evidence-kind identifiers to control identifiers
> in two or more compliance frameworks; a renderer interface admitting
> framework-specific renderer modules that select log entries by
> evidence-kind and crosswalk projection and assemble them into a
> framework-shaped evidence package; an inclusion-proof endpoint exposing
> Merkle proofs of inclusion for content addresses in the log.**

### Prior-art gap
Existing tamper-evident logs (Rekor, Trillian, QLDB) are general-purpose.
Continuous-compliance platforms render reports but from monitoring data, not
from signed physical-maintenance events organized by canonical evidence-kind
identifiers.

### Commercial leverage
Cuts the marginal cost of adding the (N+1)th framework to "write one renderer
plus crosswalk row."

---

## P-6 — Method: Compliance-Aware Re-Routing of Maintenance Work to a Different Substrate

### Problem
When a maintenance step requires capabilities the current substrate cannot
attest to (e.g. FIPS HSM offline), today the work either stalls or is
performed and audited later.

### Novelty
At compile time, when the SCA does not satisfy a control, the policy engine
**enumerates alternative substrates whose SCAs do satisfy it** and rewrites
the procedure DAG to dispatch affected steps there, recording the rewrite in
the compile trace.

### Claim sketch (dependent on P-2)
> **The method of [P-2], further comprising: responsive to a control in the
> compliance envelope being unsatisfied by the substrate capability
> attestation, querying a substrate registry for an alternative substrate
> whose attestation satisfies the control; rewriting the procedure DAG to
> dispatch the unsatisfied steps to the alternative substrate; recording the
> rewrite and the alternative substrate's identity in the compile trace.**

---

## P-7 — Method: Differential-Privacy Budget as a Billable Resource

### Problem
DP budgets in federated learning are typically modeled as engineering
constants. They are not exposed as a resource that operators can buy more of
or trade off against feature quality.

### Novelty
Treat the DP budget as a **first-class metered resource** on the MaaS
service contract. The federated learner records budget consumption per
contribution; the billing reconciler aggregates consumption per contract
period; tenants can opt to consume more budget (tighter noise) for a price
or accept looser noise to save budget.

### Claim sketch (dependent on P-3)
> **The method of [P-3], further comprising: maintaining a per-tenant
> differential-privacy budget account; debiting the account by the budget
> consumed by each contribution; reading the per-period account balance into
> a billing reconciler tied to a service contract; emitting an invoice line
> for differential-privacy budget consumed in the period.**

---

## Prioritization for the provisional

Concepts P-1 through P-5 will be included in the single Provisional Patent
Application (`provisional-patent-application.md`). P-6 and P-7 are described
in the specification so that priority is preserved, but their formal claims
can be deferred to the non-provisional or continuations within the 12-month
window.

## Strategic notes

- **First-to-file**: the US is first-inventor-to-file under the AIA. The
  provisional anchors a priority date; the non-provisional must follow
  within 12 months.
- **Foreign filing**: most non-US jurisdictions require *absolute novelty*.
  Avoid any public disclosure (talks, demos, blog posts, GitHub publication)
  before filing if foreign protection is desired.
- **PCT route**: file a PCT within 12 months of the provisional to preserve
  international options.
- **Inventorship**: must be accurate; include each natural person who
  contributed to conception of at least one claim.
- **Defensive posture**: P-5 (evidence ledger + renderers) is a candidate for
  defensive publication if budget for full prosecution is tight — it is the
  most likely to be independently re-invented.
