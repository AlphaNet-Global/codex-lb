## 1. Patch existing dependencies

- [x] 1.1 Update only the urllib3 minimum/lock and exact rustls workspace pin/lock to their patched releases.
- [ ] 1.2 Validate both locks, strict OpenSpec change and affected remote CI, including native tests, cargo-deny and Trivy.
- [ ] 1.3 Merge the passing change and verify main image publication.
