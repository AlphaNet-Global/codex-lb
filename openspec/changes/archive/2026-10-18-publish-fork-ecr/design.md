## Context

This records the implemented fork publication decisions from the proposal and context; it does not introduce a new deployment mechanism. The fork needs a private image after its existing complete CI succeeds.

## Goals / Non-Goals

Publish the exact validated main revision using temporary AWS credentials and return an immutable digest. Application deployment, public releases, PyPI and branch protection are managed separately.

## Decisions

Use a fork-guarded `workflow_run` job with explicit successful push/main/same-repository conditions. Check out the triggering head SHA, reuse the existing Dockerfile/main cache, and tag with SHA/run/attempt. The job uses GitHub OIDC, a repository-scoped ECR role and account verification; no stored AWS keys. Reusing upstream release automation would invoke unrelated public publication steps.

## Risks / Trade-offs

A maintainer can still change workflow code while branch protection is deferred; the AWS role limits repository scope. Images are rebuilt after validation, so the output digest is read back from ECR and pinned independently in platform-devops. Disabling this workflow stops future publication without deleting deployed images or database data.
