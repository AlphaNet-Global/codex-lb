# Internal EKS image publication

This workflow is for the AlphaNet-Global fork. It starts after all existing CI gates pass on main, including the Docker security scan. It rebuilds the same revision using the existing Dockerfile and reuses the trusted main build cache. It does not invoke the upstream release workflow.

AWS setup belongs to platform-devops: repository `internal/codex-lb`, region `ap-southeast-1`, account `170033428404`, and role `AlphaNetPlatformWorkload-CodexLbEcrPush`. The role trusts the repository's verified immutable OIDC subject on main; the repository variable `AWS_ECR_ROLE_ARN` references it. There are no AWS secret keys in GitHub.

The job summary provides `registry/repository@sha256:...`; platform-devops promotes this exact reference. Reruns get a distinct attempt tag. A later main commit does not change which revision a completed CI event publishes.

Branch protection is a separately deferred organization decision. This workflow enforces successful CI for publication, but does not itself prevent someone with write access from changing main or its workflows. The role is limited to the codex-lb image repository.

Publication verification is complete; the exact successful run and digest are recorded below.

Local verification: actionlint passed; `openspec validate publish-fork-ecr --strict` passed. `openspec validate --specs` reports 50 passing and 17 existing failures in unchanged main specs (for example, account-routing requirement 24 lacks SHALL/MUST). No application files or existing specs changed. A local Docker build could not fetch Docker Hub authorization because of a network EOF; the later cloud CI and real image publication supply the completed evidence below.

## Completed verification (2026-10-18)

[Security patch PR CI](https://github.com/AlphaNet-Global/codex-lb/actions/runs/39113640473), [complete merged-main CI](https://github.com/AlphaNet-Global/codex-lb/actions/runs/39114887385), and [actual OIDC ECR publication](https://github.com/AlphaNet-Global/codex-lb/actions/runs/39116001920) passed. Main CI includes Python unit/integration/PostgreSQL, Rust build/native probes/cargo-deny, Docker/Trivy, frontend, migrations, Helm validation and kind smoke. Source revision is `a695de9cc024a061931a5799c6b895680cca7735`.

Published tag: `a695de9cc024a061931a5799c6b895680cca7735-39116001920-1`. ECR digest: `sha256:44b703f2c3df96f76d5fdf38a736adcbc33d1ee64bf37ec8e2f32d62700f028f`; its linux/amd64 manifest was read back independently. The earlier failed CI event produced a skipped publication run (39112130347), without running AWS credential steps. The workflow guards reject PR/foreign-repository events; no synthetic foreign-repository event was executed.

The platform repository separately pinned this digest and verified application migration/schema gate, dashboard authentication, telemetry, administrator persistence after Pod replacement, and retirement of the manually configured setup token. These deployment facts do not certify upstream provider authorization, model requests or BestProxy, which remain separate platform acceptance items.

No security scan was bypassed. Local Cargo/full Python suites were not repeated; the remote jobs provide their evidence. Existing unrelated main-spec validation failures and unconfigured upstream Pages/release automation remain outside these two changes.
