# Deployment notes

## Repo layout for deployable artifacts (planned)

```
infra/
├── profiles/
│   ├── commercial/      # Helm values + Terraform
│   ├── fedramp-mod/
│   ├── fedramp-high/
│   ├── itar-onprem/
│   └── edge-nercip/
├── modules/             # shared Terraform modules
└── policies/            # OPA bundles emitted from compliance/
```

## Capability attestation contract

Every substrate (cloud region, edge appliance, enclave) MUST expose a signed
JSON document — the **Substrate Capability Attestation (SCA)** — declaring:

- FIPS mode (boolean, with module cert numbers)
- KMS / HSM identity
- Network egress class (`open` | `restricted` | `air-gapped` | `one-way`)
- Identity providers wired
- Data residency region (ISO 3166-2)
- Hardware root-of-trust (TPM / SEV-SNP / Nitro / TDX)
- Audit log sink (URL or sneakernet declaration)

The policy engine consumes the SCA when compiling a work order. A step that
requires `kms.fips=true` and finds `kms.fips=false` either (a) refuses to
compile, (b) compiles with an additional manual evidence step, or (c)
re-routes the work order to a different substrate, depending on framework
policy.

This SCA-conditioned compilation is part of claim 2.
