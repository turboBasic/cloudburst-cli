# Cloudburst project instructions

Cloudburst is a standalone Go CLI for running one heavy-compute job on a disposable VM. The first providers are AWS and Hetzner Cloud. The architecture and product decisions are tracked in [Issue #1](https://github.com/turboBasic/cloudburst-cli/issues/1).

## Stack and structure

- Go CLI; one Go module for the deployable binary.
- AWS and Hetzner provider SDKs, behind small provider interfaces.
- `mise.toml` pins local tools; `justfile` is the task interface; `prek` owns lint and formatting hooks.
- CI should run lint, tests, and build only. It must not create cloud resources.

```text
.
├── .github/                 # GitHub automation and Copilot instructions
├── cmd/cloudburst/          # CLI entry point
├── docs/                    # Project instructions and design records
└── internal/                # Private implementation packages
```

## Go rules

- Return actionable errors and wrap causes with `%w`; do not panic for operational failures.
- Pass dependencies explicitly; do not add mutable package-level state.
- Format with `gofmt` and `goimports`. Exported identifiers need useful doc comments.
- Put tests beside the code and use table-driven cases where behaviour has multiple cases.
- Keep one `go.mod` for this binary. Do not add dependencies for small standard-library tasks.

## Security and safety

- Read secrets from explicitly selected environment variables. Never print secret values or put them in command-line arguments, cloud-init, or logs.
- Never create, modify, or destroy live cloud resources while developing or running CI unless the user explicitly requests that operation.

## Repository workflow

Use `just` for project tasks. Run `just lint` after code or configuration changes. `just test` and `just build` are available for implementation work and CI. Keep provider credentials out of repository files; use environment variables or workload identity in CI.
