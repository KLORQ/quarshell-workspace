# Production gate evidence — 2026-10-24

This record supersedes earlier run notes that reported hosted KVM as
unverified. It records workflow acceptance only; it does not promote aggregate
OpenShell compatibility features to `pass`.

## Commits under validation

- Product repository `KLORQ/quarshell`: `5930d749385ebd97f71e8cb0dd4eecd3ecedc7b4`
- Engineering workspace `KLORQ/quarshell-workspace`:
  `24675f9879eef42caf2d4545b826cedcd4feaf96`

Both local `main` branches matched `origin/main`, and both worktrees were clean
after the fixes were pushed.

## GitHub Actions results

| Repository | Run | Result | Scope |
|---|---:|---|---|
| `quarshell` | [40110712431](https://github.com/KLORQ/quarshell/actions/runs/40110712431) | pass | Linux build/test, docs, Docker/Podman E2E, Gateway image |
| `quarshell` | [40110712440](https://github.com/KLORQ/quarshell/actions/runs/40110712440) | pass | Linux KVM, macOS HVF, Windows MXC platform jobs |
| `quarshell` | [40110738524](https://github.com/KLORQ/quarshell/actions/runs/40110738524) | pass | Release artifact build and checksum verification; publish skipped on `main` |
| `quarshell-workspace` | [40110357702](https://github.com/KLORQ/quarshell-workspace/actions/runs/40110357702) | pass | Linux build/test, docs, Docker/Podman E2E, Gateway image |
| `quarshell-workspace` | [40110152578](https://github.com/KLORQ/quarshell-workspace/actions/runs/40110152578) | pass | macOS build; signing skipped |
| `quarshell-workspace` | [40110152762](https://github.com/KLORQ/quarshell-workspace/actions/runs/40110152762) | pass | Linux KVM, macOS HVF, Windows MXC platform jobs |
| `quarshell-workspace` | [40110738599](https://github.com/KLORQ/quarshell-workspace/actions/runs/40110738599) | pass | Release artifact build and checksum verification; publish skipped on `main` |

The live KVM job completed the runtime build and lifecycle acceptance. Windows
MXC and macOS HVF jobs passed their configured build/capability and lifecycle
checks. A green platform workflow does not establish the full product parity
for those backends.

## Workspace inventory gates

On the workspace checkout, the following passed on 2026-10-24:

- `task compat:inventory` — 1,972 upstream files, 29 features, 2,087 coverage
  items, 10 Go modules; driver config inventory reports 5 verified and 49
  partial fields.
- `task evidence:check` — 29 rows valid: 1 pass and 28 unverified.
- `task evidence:report-check` — generated status is current.

The remaining rows stay open until their feature-level positive and negative
acceptance runs exist. These workflow results do not prove live Kubernetes,
GPU/VFIO, full Windows MXC runtime parity, complete Gateway-to-VM feature
coverage, published consumer installation, image publication, or macOS
notarization.
