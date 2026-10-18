# Fork image publication

The internal publication workflow follows successful same-repository push CI on main, checks out that exact revision and publishes linux/amd64 to the dedicated private ECR repository. The tag contains SHA/run/attempt, and the job reads back the immutable digest for platform-devops. Credentials come from GitHub OIDC through `AWS_ECR_ROLE_ARN`; AWS setup belongs to platform-devops.

This does not deploy Kubernetes resources or invoke upstream public-release/PyPI automation. Branch protection and migration from temporary personal Git repository credentials remain separate platform work.

Implementation and completed publication evidence are in [the archived change](../../changes/archive/2026-10-18-publish-fork-ecr/context.md).
