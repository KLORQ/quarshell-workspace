# Symbolic prover fail-closed result handling — 2026-10-24

## Scope

`SymbolicReport` now distinguishes a solver capability gap from an uncertain
answer. Errors matching `domain.ErrUnsupported` produce the explicit
`Unsupported` relation; timeouts and other solver errors produce
`Inconclusive`. Only `Within` is allowed by `Report.Allows`.

Source:

- `quarshell-policy/prover/symbolic.go`
- `quarshell-policy/prover/solver_test.go`

## Verification

Passed:

```sh
go test ./prover
task test
```

The new test injects an unsupported solver error and a timeout error and checks
that neither result can allow the policy. The full ten-module workspace test
suite passed.

## Remaining scope

This does not establish complete OpenShell prover parity. Bounded containment
matcher coverage, counterexample completeness, risk analysis parity and solver
timeout behavior across all queries still require acceptance. `OS-PROVER`
remains `unverified`.
