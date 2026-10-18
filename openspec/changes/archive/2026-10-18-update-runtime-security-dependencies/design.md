## Context

This records the implemented patch choice from the proposal and advisories. Existing CI rejects urllib3 2.7.0 and rustls 0.23.45 before image publication.

## Goals / Non-Goals

Resolve the patched compatible dependencies while preserving runtime interfaces, frozen locks and existing scanner policies. No proxy feature, API, database schema or unrelated dependency upgrade is included.

## Decisions

Raise the existing urllib3 minimum/lock to 2.8.0 and exact rustls workspace/lock pin to 0.23.46; update the corresponding packaging assertion. Rust package dependency/features/MSRV metadata remain equal. Using new libraries or scanner exclusions would expand the change without addressing the existing build contract.

## Risks / Trade-offs

Local Cargo and the project Python environment are unavailable; complete remote tests, native probes, cargo-deny and Docker/Trivy provide verification. Rollout uses the verified ECR digest in platform-devops; rolling back to a vulnerable dependency is not an acceptable security remediation. No database migration is introduced by this patch.
