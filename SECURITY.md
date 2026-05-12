# Security Policy

CVMA targets regulated environments (FedRAMP, CMMC, NIST 800-171, HIPAA, ITAR,
NERC-CIP, NIS2). Vulnerability reports are treated accordingly.

## Reporting a Vulnerability

**Do not open a public GitHub issue for security problems.**

Report via GitHub's private vulnerability reporting:
<https://github.com/khaaliswooden-max/cvma/security/advisories/new>

Include:

- A description of the issue and the component affected (e.g. `src/policy-engine`).
- Steps to reproduce, or a proof-of-concept.
- The impact you believe it has (confidentiality, integrity, availability,
  compliance posture).
- Any known mitigations or workarounds.

If the report concerns export-controlled material (ITAR / EAR), mark it as
such in the first line of the report and omit technical detail that would
itself constitute a controlled disclosure; coordinate an out-of-band channel
before sharing specifics.

## Response Expectations

| Stage | Target |
| --- | --- |
| Acknowledgement | 3 business days |
| Initial assessment | 10 business days |
| Fix or mitigation plan | 30 business days for high/critical |

These are targets for a pre-alpha project and are not contractual SLAs.

## Scope

In scope:

- Code under `src/`.
- Architecture and policy artifacts under `design/`, `compliance/`, `infra/`
  where a defect would propagate to deployed systems built from them.

Out of scope:

- Third-party dependencies — report upstream.
- Findings that require physical access or already-compromised credentials,
  unless they demonstrate a missing control CVMA claims to provide.

## Disclosure

Coordinated disclosure. We will credit reporters who wish to be named once a
fix is available and downstream users have had a reasonable window to update.
