# Prior-Art Survey — CVMA Claim Space

**Scope.** This survey covers the USPTO patent, USPTO published-application,
academic, and open-source landscape relevant to the Cross-Vertical Maintenance
Systems Architecture (CVMA). It is intended to establish the state of the
art against which the patent ideation in `patent-ideation.md` is judged for
non-obviousness.

**Method.** Searches conducted via USPTO Patent Public Search (PPUBS),
Google Patents, Justia, and PatSnap-indexed corpora; supplemented by IEEE,
Springer, MDPI, and arXiv for academic context; OSS projects identified via
GitHub topics and standards body publications. Search-string families
included: digital-twin + predictive maintenance, federated learning +
maintenance + multi-tenant, RUL + transfer learning, ontology + cross-domain
maintenance, compliance + automated workflow, asset administration shell,
NSN / MIL-STD substitution.

## 1. Digital twin and predictive maintenance

| Cite | Type | Summary | Distance from CVMA |
| --- | --- | --- | --- |
| **US 2016/0247129 A1** — "Digital twins for energy efficient asset maintenance" | App. | Couples a digital-twin model of an asset to predictive maintenance scheduling for energy assets. | Single-vertical (energy); no compliance compilation; no federated cross-vertical learning. |
| **US 2017/0286572 A1** — "Digital twin of twinned physical system" | App. | General digital-twin framework for representing a physical system and its operational state. | Foundation only; does not address cross-vertical canonical ontology nor compliance-conditioned procedures. |
| **CN 112418523 A** — "Digital twin-based after-sales equipment predictive maintenance coordination system" | App. | After-sales coordination using digital-twin signals. | Closed-tenant; coordination scope, not architecture. |
| **arXiv 2509.24443** — Systematic review of digital-twin-driven predictive maintenance | Lit. | Taxonomy of DT-PdM architectures; identifies cross-domain transfer and standards gap as open problems. | Confirms the gap CVMA targets. |
| **MDPI Machines 2025, 13(6):481** — Data-driven DT framework for smart manufacturing | Lit. | DT-PdM for manufacturing; AAS-aligned. | Single-vertical. |

Volume context: digital-twin filings increased ~600% from 2017 to 2025 with
~2,451 applications in 2025 (PatSnap landscape, 2026). The space is crowded
but vertical-specific; **cross-vertical** filings are sparse.

## 2. Asset Administration Shell (AAS) and CMMS / EAM platforms

| Cite | Type | Summary | Distance from CVMA |
| --- | --- | --- | --- |
| **MDPI Sensors 20(21):6028** — "A model for predictive maintenance based on AAS" | Lit. | Uses AAS submodels for PdM. | Industrie 4.0 only; no compliance compilation. |
| **MDPI Sensors 25(7):1978** — AAS tool comparison (Eclipse BaSyx, FA3ST, AASX server, NOVAAS) | Lit. | Benchmarks open-source AAS implementations. | Establishes that AAS is the leading open-source twin substrate, not a competing architecture. |
| **Eclipse BaSyx** (OSS) | OSS | Open-source AAS server, broker, components. | Candidate dependency for CVMA's twin layer; does not address claims 1–5. |
| **openMAINT / CalemEAM / Atlas CMMS / SuperCMMS** | OSS | Open-source CMMS / EAM platforms. | Single-vertical CMMS; would be candidate downstream consumers of the CVMA CAG, not equivalents. |
| **CARL e-Twin** (commercial) | Comm. | Commercial CMMS with digital-twin features. | Single-vertical commercial product. |

## 3. Federated learning for predictive maintenance

| Cite | Type | Summary | Distance from CVMA |
| --- | --- | --- | --- |
| **US 12,488,223 B2** — "Federated learning for training machine learning models" | Pat. | FL with multi-tenant resource pooling. | Generic FL infrastructure; no cross-vertical embedding alignment; no compliance-keyed DP budget. |
| **US 12,033,074 B2** — "Vertical federated learning with compressed embeddings" | Pat. | Vertical FL across parties holding different features for the same entities (e.g., manufacturer + end-user). | Closest hit to CVMA claim 3 in motivation. Differs: same-entity vertical FL, not cross-vertical FL across different entities sharing failure modes; no canonical-ontology alignment loss; no envelope-keyed DP. |
| **US App. 2023/0222356** — "Federated learning method" | App. | FL task deployment scheme. | Infrastructure; no maintenance domain specifics. |
| **US App. 2021/0192078 A1** — "User behavior model development with private federated learning" | App. | Private FL on user behavior. | Adjacent domain (behavior, not maintenance). |
| **US App. 2022/0101206** — "Federated learning mechanism" | App. | General mechanism. | Generic. |
| **arXiv 1909.07053** — Transfer learning for RUL via consensus self-organizing models | Lit. | TL for RUL; non-federated. | Establishes TL technique, not federated cross-vertical. |
| **Springer J. Intell. Manuf. (2024)** — Pre-trained model selection for TL of RUL | Lit. | Grinding-wheel RUL TL. | Single-asset-class. |
| **Springer Discov. App. Sci. (2025)** — Privacy-preserving PdM for cross-border unmanned logistics with FL + blockchain | Lit. | FedProx + ZKP + FHE + LDP + sharding. | Heavy cryptographic stack; single logistics domain; not cross-vertical via ontology alignment. |

## 4. Maintenance ontologies / cross-domain asset normalization

| Cite | Type | Summary | Distance from CVMA |
| --- | --- | --- | --- |
| **CDM-Core (CREMA project)** | Lit. | Manufacturing ontology in OWL2 for production + maintenance. | Single-vertical (manufacturing). |
| **Springer J. Intell. Manuf. (2024)** — Review of manufacturing ontologies | Lit. | Surveys ontologies; identifies cross-industry reuse as open. | Confirms gap. |
| **Emerald ECAM (2025)** — Unified ontology framework for cross-domain integration | Lit. | Cross-domain integration ontology in construction asset mgmt. | Construction-anchored; no compliance-conditioned procedures; no FL. |
| **SERC Handbook on Digital Engineering with Ontologies v2 (2025, DoD CTO)** | Lit./Std. | DoD-aligned ontology approach to digital engineering. | Sets DoD expectations; complements CVMA's RSI pillar. |
| **ISO 14224, IEC 81346, MIMOSA CCOM, ISA-95, IEC 62264, HL7 FHIR Device, BACnet, Project Haystack, AAS submodels** | Std. | Per-vertical reference standards. | CVMA's CAG is a *unification* across these; none of them does that on its own. |

## 5. Compliance automation and policy-driven workflows

| Cite | Type | Summary | Distance from CVMA |
| --- | --- | --- | --- |
| **US 8,271,615 B2** — Centrally managing and monitoring SaaS applications | Pat. | SaaS management/monitoring. | Generic; not maintenance-specific. |
| Commercial: Vanta, Drata, Paramify, Rizkly, Cuick Trac, Hyperproof | Comm. | Continuous-compliance evidence platforms (SOC 2, ISO 27001, NIST 800-171, CMMC). | Compliance monitoring of IT estates; not coupled to physical maintenance task compilation. |
| **OPA (Open Policy Agent)** | OSS | General-purpose policy engine. | Candidate dependency; doesn't supply the maintenance-specific control crosswalk or procedure-DAG compilation. |
| **OSCAL (NIST)** | Std./OSS | Open Security Controls Assessment Language. | Candidate format for `compliance/frameworks/*.yaml`; doesn't address compilation into maintenance procedures. |

No US patent located that compiles a maintenance procedure DAG by combining
(a) a canonical asset model, (b) a multi-framework control crosswalk, and (c)
a runtime substrate capability attestation, with a signed compile trace.

## 6. RSI / part substitution

| Cite | Type | Summary | Distance from CVMA |
| --- | --- | --- | --- |
| **CJCSI 2700.01H, DoDI 4120.24, DoDI 2010.07, NATO RSI reference book (DTIC ADA151461)** | Std. | RSI doctrine for coalition standardization. | Doctrinal; supplies the policy framing CVMA's RSI layer encodes. |
| **DLA WebFLIS / NSN catalog, GSA Advantage, ASSIST** | DB | Public databases for parts/standards. | Data sources, not architecture. |
| Commercial cross-reference catalogs (SKF, NSK, Bosch, Caterpillar, etc.) | Comm. | OEM substitution tables. | Lookup tables. |

No US patent located that derives an RSI substitution by reasoning over a
unified maintenance ontology, validates the substitution against an asset's
compliance envelope, and auto-emits a signed evidence pack.

## 7. Tamper-evident audit / evidence logs

| Cite | Type | Summary | Distance from CVMA |
| --- | --- | --- | --- |
| **Certificate Transparency (RFC 9162), Sigstore Rekor, Google Trillian, AWS QLDB, Azure Confidential Ledger** | OSS / Comm. | Append-only Merkle logs. | Candidate substrates; not specialized to per-framework evidence rendering from a canonical maintenance log. |
| Generic SIEM platforms (Splunk, Elastic, Sentinel) | Comm. | Log aggregation and audit. | Not signed-by-source; not maintenance-domain renderers. |

## 8. Identified gaps (the white space)

Across the surveyed corpus we did not locate prior art that combines **all** of:

1. A **vertical-agnostic canonical asset graph** populated by a **retrieval-grounded, confidence-scored, HITL-reconciled mapper** with **per-record provenance** anchored to a tamper-evident log.
2. A **compliance-conditioned procedure-DAG compiler** that takes a canonical maintenance task, an asset's compliance envelope, and a **substrate capability attestation** as inputs and emits a signed compile trace.
3. **Federated learning that transfers RUL knowledge across verticals** by aligning embeddings around shared FailureModes in the canonical graph, with **DP budget keyed to the asset's compliance envelope** and that budget itself logged as evidence.
4. **RSI substitution derived by ontology reasoning** that is **gated by the compliance envelope** and **auto-emits an evidence pack** with standard citations and residual-risk delta.
5. **A canonical evidence ledger with per-framework renderers** projecting the same signed events into FedRAMP, CMMC, HIPAA, NERC CIP, ISO 27001, NIS2, and 21 CFR Part 11 evidence shapes.

Individually, components are known; the **combination and the inter-component
information flows** (envelope propagated from CAG into procedure compilation,
DP accounting, and substitution gating) are where the non-obviousness lies.
This is the surface attacked in `patent-ideation.md` and claimed in
`provisional-patent-application.md`.

## Sources

- [USPTO PPUBS — Patent Public Search](https://ppubs.uspto.gov/pubwebapp/)
- [Drafting a Provisional Application — USPTO (Jun 2023)](https://www.uspto.gov/sites/default/files/documents/provisional-applications-6-2023.pdf)
- [MPEP 601 — Content of Provisional and Nonprovisional Applications](https://www.uspto.gov/web/offices/pac/mpep/s601.html)
- [US 2016/0247129 A1 — Digital twins for energy efficient asset maintenance](https://patents.google.com/patent/US20160247129A1/en)
- [US 2017/0286572 A1 — Digital twin of twinned physical system](https://patents.google.com/patent/US20170286572A1/en)
- [CN 112418523 A — Digital twin after-sales predictive maintenance coordination](https://patents.google.com/patent/CN112418523A/en)
- [US 12,488,223 B2 — Federated learning for training machine learning models](https://patents.google.com/patent/US12488223B2/en)
- [US 12,033,074 B2 — Vertical federated learning with compressed embeddings](https://patents.google.com/patent/US12033074B2/en)
- [US Pub. 2023/0222356 — Federated learning method](https://patents.justia.com/patent/20230222356)
- [US 2021/0192078 A1 — User behavior model with private federated learning](https://patents.google.com/patent/US20210192078A1/en)
- [US Pub. 2022/0101206 — Federated learning mechanism](https://patents.justia.com/patent/20220101206)
- [US 8,271,615 B2 — Centrally managing and monitoring SaaS applications](https://patents.google.com/patent/US8271615B2/en)
- [PatSnap — Digital twin tech landscape for manufacturing 2026](https://www.patsnap.com/resources/blog/articles/digital-twin-tech-landscape-for-manufacturing-2026/)
- [arXiv 2509.24443 — Systematic review of digital-twin-driven predictive maintenance](https://arxiv.org/html/2509.24443v1)
- [MDPI Machines 13(6):481 — Data-driven DT framework for smart manufacturing](https://www.mdpi.com/2075-1702/13/6/481)
- [MDPI Sensors 20(21):6028 — Predictive maintenance based on AAS](https://www.mdpi.com/1424-8220/20/21/6028)
- [MDPI Sensors 25(7):1978 — AAS tool comparison](https://www.mdpi.com/1424-8220/25/7/1978)
- [Springer SAM (2024) — Digital twin and the AAS](https://link.springer.com/article/10.1007/s10270-024-01255-0)
- [PMC PMC11174398 — RUL prediction based on deep learning survey](https://pmc.ncbi.nlm.nih.gov/articles/PMC11174398/)
- [arXiv 1909.07053 — Transfer learning for RUL](https://arxiv.org/abs/1909.07053)
- [Springer JIM (2024) — Pre-trained model selection for TL of RUL](https://link.springer.com/article/10.1007/s10845-023-02154-9)
- [Springer Discov. App. Sci. (2025) — FL + blockchain PdM for cross-border logistics](https://link.springer.com/article/10.1007/s42452-025-07980-5)
- [Springer JIM (2024) — Review of manufacturing ontologies](https://link.springer.com/article/10.1007/s10845-024-02425-z)
- [Emerald ECAM — Unified ontology framework for cross-domain integration](https://www.emerald.com/ecam/article/33/3/1784/1332353/A-unified-ontology-framework-for-cross-domain)
- [DoD CTO — SERC Handbook on Digital Engineering with Ontologies v2 (2025)](https://www.cto.mil/wp-content/uploads/2025/06/SERC_Handbook-on-Digital-Engineering-with-Ontologies_2.0.pdf)
- [CJCSI 2700.01H — RSI between US and allies](https://www.jcs.mil/Portals/36/Documents/Library/Instructions/CJCSI%202700.01H.pdf)
- [DoDI 4120.24 — Defense Standardization Program](https://www.esd.whs.mil/Portals/54/Documents/DD/issuances/dodi/412024p.pdf)
- [DAU — RSI Activities reference](https://www.dau.edu/cop/dsp/documents/rationalization-standardization-and-interoperability-rsi-activities)
- [DSP — RSI activities overview](https://www.dsp.dla.mil/Portals/26/Documents/Publications/Conferences/2018/2018%20International%20Standardization%20Workshop/20181101-Item2-RSI-IntlStdznWorkshop_Croteau.pdf?ver=2018-11-06-152040-673)
- [Eclipse BaSyx (AAS)](https://www.eclipse.org/basyx/)
- [Atlas CMMS (OSS)](https://atlas-cmms.com/)
- [PTC — Why FedRAMP Is the Fast Lane to CMMC](https://www.ptc.com/en/blogs/aerospace-and-defense/why-fedramp-is-the-fast-lane-to-cmmc)
- [MindPoint — NIST 800-53 / 800-171 / CMMC / FedRAMP quick guide](https://www.mindpointgroup.com/blog/a-quick-guide-to-nist-800-53-nist-800-171-and-cmmc-and-fedramp)
