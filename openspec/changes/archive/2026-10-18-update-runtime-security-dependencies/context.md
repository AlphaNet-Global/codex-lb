# Why this patch is part of initial deployment

Main CI run 39111107078 built the fork image, then rejected installed urllib3 2.7.0. The upstream advisories identify 2.8.0 as the fixed version:

- [HTTPS proxy TLS configuration override](https://github.com/urllib3/urllib3/security/advisories/GHSA-8988-9cw3-xx77): CVE-2026-97687.
- [Unbounded chunk-size buffering](https://github.com/urllib3/urllib3/security/advisories/GHSA-vxq7-64xx-v4gw): CVE-2026-97689.

The project already has an explicit urllib3 minimum. Updating that minimum and its existing lock entry is sufficient; no new HTTP client, runtime option or scanner exclusion is needed. The real image scan remains the acceptance check. This does not configure BestProxy or change application egress routes.


The same complete main CI also fails cargo-deny for rustls 0.23.45. Official RustSec advisories require >=0.23.46: [0346](https://rustsec.org/advisories/RUSTSEC-2026-0346), [0347](https://rustsec.org/advisories/RUSTSEC-2026-0347), [0348](https://rustsec.org/advisories/RUSTSEC-2026-0348), [0349](https://rustsec.org/advisories/RUSTSEC-2026-0349). Crates.io index entries for 0.23.45 and 0.23.46 differ only in version, checksum and publication time; dependency/features/MSRV metadata are identical. Preserve other Cargo package entries and the native worker's existing policy. Remote pinned-toolchain builds/tests and audit are required evidence; this workstation has no Cargo installation.

PR CI 39112793535 passed the Docker/Trivy gate and Rust build, native probes and cargo-deny. Its unit suite reported 10,532 passed, with one failure because the existing release-family packaging assertion still required rustls 0.23.45. Updated that single expectation to the new exact workspace pin. Executing that unchanged function body against the actual lockfile reproduced RED and then passed GREEN locally; the subsequent complete remote pytest gate passed, as recorded below (this workstation has no project virtual environment).

## Completed verification (2026-10-18)

[Security patch PR CI](https://github.com/AlphaNet-Global/codex-lb/actions/runs/39113640473), [complete merged-main CI](https://github.com/AlphaNet-Global/codex-lb/actions/runs/39114887385), and [actual OIDC ECR publication](https://github.com/AlphaNet-Global/codex-lb/actions/runs/39116001920) passed. Main CI includes Python unit/integration/PostgreSQL, Rust build/native probes/cargo-deny, Docker/Trivy, frontend, migrations, Helm validation and kind smoke. Source revision is `a695de9cc024a061931a5799c6b895680cca7735`.

Published tag: `a695de9cc024a061931a5799c6b895680cca7735-39116001920-1`. ECR digest: `sha256:44b703f2c3df96f76d5fdf38a736adcbc33d1ee64bf37ec8e2f32d62700f028f`; its linux/amd64 manifest was read back independently. The earlier failed CI event produced a skipped publication run (39112130347), without running AWS credential steps. The workflow guards reject PR/foreign-repository events; no synthetic foreign-repository event was executed.

The platform repository separately pinned this digest and verified application migration/schema gate, dashboard authentication, telemetry, administrator persistence after Pod replacement, and retirement of the manually configured setup token. These deployment facts do not certify upstream provider authorization, model requests or BestProxy, which remain separate platform acceptance items.

No security scan was bypassed. Local Cargo/full Python suites were not repeated; the remote jobs provide their evidence. Existing unrelated main-spec validation failures and unconfigured upstream Pages/release automation remain outside these two changes.
