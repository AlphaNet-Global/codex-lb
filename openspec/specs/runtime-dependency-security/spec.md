# runtime-dependency-security Specification

## Purpose
Keep the existing Python and native runtime dependencies on the verified security patch baseline without weakening image publication gates.
## Requirements
### Requirement: Resolve patched runtime dependencies

Python package metadata SHALL require urllib3 version 2.8.0 or later, and its frozen lock SHALL satisfy that minimum. The Rust workspace and frozen Cargo lock SHALL select rustls 0.23.46 or later within the compatible 0.23 series. Docker image publication SHALL continue to require successful CI security gates without new vulnerability exclusions.

#### Scenario: Build the frozen runtime

- **WHEN** the existing Dockerfile builds the Python runtime and native egress binary using frozen dependencies
- **THEN** urllib3 resolves to 2.8.0 or later and rustls resolves to at least 0.23.46 in its existing compatible series
- **AND** Trivy and cargo-deny run with the existing security policies

