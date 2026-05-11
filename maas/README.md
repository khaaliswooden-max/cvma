# MaaS — Maintenance as a Service

The MaaS layer is the commercial surface of CVMA. It is intentionally thin: a
service contract, an SLA model, a pricing model, and a billing reconciler.
All technical decisions live below it.

## Contract model

A `ServiceContract` ties:

- a set of `Asset`s (or a query over assets, e.g. "all rooftop HVAC > 5T")
- a `ComplianceEnvelope` to enforce
- an `SLATier` (response time, MTTR target, RUL-based proactive coverage)
- a `PricingPlan` (per-asset, per-event, per-RUL-hour-recovered, hybrid)
- a `Tenant`/billing account

## SLA tiers (illustrative)

| Tier | Response | MTTR | Proactive % | DP budget | Evidence renderer set |
| --- | --- | --- | --- | --- | --- |
| Bronze   | 24h | 7d | 0%   | None     | SOC 2 |
| Silver   | 4h  | 24h | 50% | Loose    | SOC 2 + ISO 27001 |
| Gold     | 1h  | 8h | 80%  | Tight    | + HIPAA / CMMC L2 |
| Platinum | 15m | 4h | 95% | Strictest| + FedRAMP-High / NERC CIP / ITAR |

## Why the pricing model needs the patent stack

A meaningful "per-RUL-hour-recovered" pricing line requires that the system
actually produce auditable RUL estimates and counterfactual evidence that
maintenance was performed in time. That depends on the federated RUL service
(claim 3) and the tamper-evident provenance ledger (claim 5). Without those,
the price is unfalsifiable.

## Billing reconciler

Reads the provenance ledger, groups events by `ServiceContract`, applies the
`PricingPlan`, and emits invoices with cryptographic backing. Disputes can be
resolved by re-rendering the relevant evidence packs.
