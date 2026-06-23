# Changelog

## 1.0.0 — 2026-06-23

Initial public release.

- One commit per changed file with auto-generated messages
- Commit message style detection from repo's recent git log
- Multi-repo workspace support
- Secret file detection (`.env`, `*.pem`, `credentials.json`, etc.)
- Pre-commit hook compliance (no `--no-verify`)
- No `Co-Authored-By` or AI attribution trailers
- Sanity check when more than 20 files are changed
