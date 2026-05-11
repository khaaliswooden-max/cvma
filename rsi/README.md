# RSI — Rationalization, Standardization, Interoperability

NATO and the DoD use **RSI** (per CJCSI 2700.01H) as the discipline of making
allied materiel, doctrine, and logistics interchangeable. CVMA borrows the
concept and extends it across the commercial–defense boundary.

## What CVMA's RSI layer does

The RSI module attaches a **substitution graph** to the Canonical Asset
Graph. Given a part, procedure, tool, or qualification, the substitution
graph proposes equivalents that come from:

1. **Coalition-aligned defense standards** — STANAGs, MIL-STDs, DEF STANs, AECTPs
2. **Commercial-equivalent standards** — ANSI, ISO, ASTM, SAE, IEC
3. **OEM cross-reference tables** — manufacturer part substitution catalogs
4. **Federal supply chain** — NSN ↔ commercial CAGE / GTIN bridging
5. **Tenant-private substitutions** — engineering-approved overrides

## Why it's part of the patentable claim set

Existing CMMS systems hold part substitution tables. The novel element here
(see provisional, claim 4) is:

- The substitution is **proposed by reasoning over the CAG**, not by lookup —
  a substitution is valid only if (a) the candidate's failure-mode coverage
  is a superset of the original, (b) the candidate is permitted under the
  asset's compliance envelope, and (c) the substitution event auto-generates
  an evidence pack (engineering rationale, regulatory citation, residual-risk
  delta) signed and anchored to the provenance ledger.

That coupling — RSI substitution gated by compliance envelope and auto-emitting
audit-grade evidence — is the non-obvious step.

## Inputs to the RSI graph (planned ingestion)

| Source | Access | Notes |
| --- | --- | --- |
| DLA WebFLIS / NSN catalog       | Public      | NSN → part attribute |
| DoDIC                            | Public      | DoD identification codes |
| GSA Advantage                    | Public      | Commercial equivalents |
| ASSIST (MIL-STDs / STANAGs)      | Public      | Standards corpus |
| ISO / IEC / ASTM / SAE catalogs  | Licensed    | Commercial standards |
| Vendor cross-reference catalogs  | Per-tenant  | E.g. SKF/NSK bearing cross-refs |
