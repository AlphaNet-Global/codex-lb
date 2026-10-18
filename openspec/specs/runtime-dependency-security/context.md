# Runtime dependency security baseline

The deployed initial fork baseline resolves urllib3 2.8.0 and rustls 0.23.46 through the existing frozen locks. Trivy and cargo-deny retain their existing policies; no scanner exclusion was added. The patch changes no application API, schema, settings or proxy-routing interface.

The complete remote CI covers the native build/probes and Python runtime; local lack of Cargo or the project virtual environment is not treated as equivalent test evidence. Deployment pins the separately verified ECR digest through platform-devops.

Advisories, compatibility comparison and complete CI/publication evidence are in [the archived change](../../changes/archive/2026-10-18-update-runtime-security-dependencies/context.md).
