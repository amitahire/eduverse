---
name: Repository Hygiene & DX Baseline
version: 1.0.0
description: "A microagent for enforcing repository hygiene and developer experience standards."
agent: "CodeActAgent"
triggers:
  - "repo:bootstrap"
  - "setup-devx"
---

# Repository Hygiene & DX Baseline

- Prefer deterministic tools; pin versions where possible.
- Use `pre-commit` for consistent local hooks and to mirror CI checks.
- Enforce Conventional Commits for clean history and changelogs.
- Never commit secrets; run `gitleaks` locally and in CI.
- Keep `README.md` accurate. The CI job will PR updates to README’s “Changelog” and regenerate `CHANGELOG.md` from commits.
- When adding a language, wire its linter/formatter/test target into:
  - `.pre-commit-config.yaml`
  - `.github/workflows/ci.yml`
  - `Makefile` targets (`lint`, `fmt`, `test`)

## Language policies

**Python**
- `ruff` for lint/format, `pytest` for tests. Prefer `uv` or `pip-tools` to lock deps.

**JS/TS**
- `eslint` + `prettier`, `jest` (or `vitest`), `npm ci` for reproducible installs.

**Go**
- `go fmt`, `go vet`, `go test ./...`, Go 1.22+.

**Rust**
- `cargo fmt --check`, `cargo clippy -D warnings`, `cargo test`.

**C/C++**
- `clang-format --Werror`, `cmake`/`ctest` minimal smoke build.

**Docs**
- `markdownlint` for `.md`; keep the top-level README authoritative.

## Commit/merge discipline

- All PRs must pass `pre-commit` and CI.
- CI pushes doc updates via a PR using `git-cliff` changelog generation.
