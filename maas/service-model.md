# MaaS service model

## Lifecycle

```
draft  -->  quoted  -->  active  -->  in-grace  -->  terminated
                  ^                              |
                  +------- renewed --------------+
```

Each state transition is a signed event on the provenance ledger so that
downstream billing can replay state deterministically.

## Pricing inputs that the architecture makes auditable

| Input | Source | Why CVMA is differentiated |
| --- | --- | --- |
| Asset count active in period | Canonical Asset Graph | Same asset modeled once across verticals |
| Work orders executed         | Provenance ledger     | Cryptographic execution proof |
| RUL hours recovered          | Federated RUL service | Counterfactual modeled per asset |
| Compliance evidence packs    | Evidence renderer     | Per-framework renderings billable |
| DP budget consumed           | Federated learner     | Privacy budget as a meterable resource |

DP-budget-as-a-meter is itself a candidate sub-claim — see
`ip/patent-ideation.md`.
