# Internal EKS image publication

This workflow is for the AlphaNet-Global fork. It starts after all existing CI gates pass on main, including the Docker security scan. It rebuilds the same revision using the existing Dockerfile and reuses the trusted main build cache. It does not invoke the upstream release workflow.

AWS setup belongs to platform-devops: repository `internal/codex-lb`, region `ap-southeast-1`, account `170033428404`, and role `AlphaNetPlatformWorkload-CodexLbEcrPush`. The role trusts the repository's verified immutable OIDC subject on main; the repository variable `AWS_ECR_ROLE_ARN` references it. There are no AWS secret keys in GitHub.

The job summary provides `registry/repository@sha256:...`; platform-devops promotes this exact reference. Reruns get a distinct attempt tag. A later main commit does not change which revision a completed CI event publishes.

Branch protection is a separately deferred organization decision. This workflow enforces successful CI for publication, but does not itself prevent someone with write access from changing main or its workflows. The role is limited to the codex-lb image repository.

Publication verification is pending; creating a workflow or role alone is not evidence that a usable image exists.

Local verification: actionlint passed; `openspec validate publish-fork-ecr --strict` passed. `openspec validate --specs` reports 50 passing and 17 existing failures in unchanged main specs (for example, account-routing requirement 24 lacks SHALL/MUST). No application files or existing specs changed. A local Docker build could not fetch Docker Hub authorization because of a network EOF; cloud CI and the real image publication remain required.
