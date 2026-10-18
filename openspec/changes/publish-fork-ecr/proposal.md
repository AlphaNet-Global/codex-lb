## Why

The AlphaNet-Global fork needs a private ECR image for its EKS deployment. The upstream release workflow also publishes Python packages and public releases, so it is unsuitable for an internal deployment build.

## What Changes

Publish the exact successful `main` push CI revision to the dedicated ECR repository using GitHub OIDC. Report an immutable digest for platform-devops. Keep the application, public release channels and database unchanged.

## Impact

One fork-scoped Actions workflow; an independently scoped AWS role and ECR repository are managed in platform-devops. No permanent AWS credentials or new application settings.
