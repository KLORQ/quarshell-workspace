# Gateway global policy history — 2026-10-24

## Scope

Implemented global `ListSandboxPolicies` and `GetSandboxPolicyStatus` reads over
the gateway-global policy revision scope. The handlers require `platform_admin`,
reject sandbox/workspace selectors when `global=true`, preserve the existing
page-size bounds, and bind pagination cursors to the selected scope. Global
status does not report a sandbox active version.

Source:

- `quarshell-gateway/internal/transport/applicationgrpc/policy_revisions.go`
- `quarshell-gateway/internal/transport/applicationgrpc/policy_test.go`

## Verification

Commands passed on 2026-10-24:

```sh
go test -C quarshell-gateway ./internal/transport/applicationgrpc
go test -C quarshell-gateway ./...
```

`TestGlobalPolicyHistoryRequiresAdminAndPaginates` covers revision ordering,
pagination, latest-status lookup, unauthenticated and non-admin rejection, and
invalid mixed global/workspace scope. The full Gateway module suite passed.

## Remaining scope

This does not close `OS-POLICY-LIFECYCLE`: global policy mutation and propagation
to sandboxes, load acknowledgement, and the complete policy draft workflow still
need implementation and acceptance. The aggregate matrix row remains
`unverified`.
