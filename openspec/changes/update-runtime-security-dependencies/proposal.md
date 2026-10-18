# Update runtime security dependencies

## Why

Complete fork CI rejects urllib3 2.7.0 for CVE-2026-97687 / CVE-2026-97689 and rustls 0.23.45 for RUSTSEC-2026-0346 through RUSTSEC-2026-0349. This prevents a validated ECR image from being published.

## What changes

Raise the existing urllib3 minimum to 2.8.0 and its frozen uv lock. Update the existing exact rustls workspace pin and Cargo lock to 0.23.46. Keep the existing Trivy and cargo-deny gates with no new exclusions. Do not upgrade unrelated dependencies.

## Impact

The existing Python runtime and native egress binary build with patched dependencies. No API, schema, settings, proxy behavior or deployment interface changes.
