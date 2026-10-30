# Cloudburst

Cloudburst is a Go CLI for running heavy-compute jobs on short-lived cloud VMs.

The project is in development.

## Development

Install the tools pinned in `mise.toml`, then use `just`:

```sh
mise install
just lint
just test
just build
```

Cloud credentials are not needed for local linting, tests, or builds.
