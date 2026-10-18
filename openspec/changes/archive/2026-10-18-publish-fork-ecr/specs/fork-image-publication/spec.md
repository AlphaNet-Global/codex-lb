## ADDED Requirements

### Requirement: Publish only a validated fork revision

The image workflow SHALL run only in `AlphaNet-Global/codex-lb` after successful completion of its `CI` workflow for a push to `main` from the same repository. It SHALL check out the completed run's exact head SHA, build the existing Dockerfile for `linux/amd64`, and publish to `170033428404.dkr.ecr.ap-southeast-1.amazonaws.com/internal/codex-lb`.

#### Scenario: Main CI succeeds

- **WHEN** a same-repository push to main completes CI successfully
- **THEN** publication builds that run's head SHA and labels the image with that revision
- **AND** it uses a unique SHA/run/attempt tag and records the ECR digest

#### Scenario: CI fails or runs on a pull request

- **WHEN** the completed CI run failed, came from another repository, or was not a push to main
- **THEN** no AWS credentials are requested and no image is published

### Requirement: Use temporary credentials scoped to this image

The publish job SHALL use GitHub OIDC and `AWS_ECR_ROLE_ARN`, verify the expected AWS account, and receive only the dedicated repository's image upload/read permissions plus regional ECR login. It SHALL NOT require stored AWS access keys, deploy to EKS, publish Python packages, or create public releases.

#### Scenario: Publication completes

- **WHEN** the image has been pushed
- **THEN** its digest is read back from ECR and validated before the job reports success
- **AND** platform-devops can pin that digest independently of source builds
