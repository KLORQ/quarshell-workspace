# Gateway sandbox configuration admission — 2026-10-24

## Implemented slice

`ReportSandboxConfiguration` now validates the authenticated sandbox scope,
active supervisor instance, admission enum, and UUID instance identity. It
persists admission state on the durable sandbox record. Pending admission can
replace an earlier instance only with the expected-instance fence. Accepted or
rejected admission must follow a pending registration for that same instance
and match the current policy version/hash, effective config revision, and
provider environment revision.

An accepted configuration is retained if a later desired update is rejected;
runtime-supplied rejection diagnostics are redacted to a Gateway-authored
message. A new sandbox runtime clears the previous admission before reporting
its own state. This follows the pinned OpenShell handler's admission ordering
and accepted-last-known-good behavior.

## Tests

The application test covers pending → accepted, stale revision rejection,
rejected-update preservation, diagnostic redaction, and clearing admission on
restart. The transport test
covers owner fencing, pending registration, mismatched revision rejection, and
accepted-state persistence. RPC fixture now expects malformed requests to fail
with `InvalidArgument`.

Passed:

```sh
go test ./internal/application/sandbox ./internal/transport/applicationgrpc
```

## Remaining scope

This does not implement configuration preparation/delivery, provisioning
deadlines and cleanup, or shared cross-replica admission state. The desired
policy/provider tuple is read before the sandbox-record write, so a concurrent
configuration mutation can race the admission check; atomic cross-record
validation and a concurrency regression are still required. `OS-API` and
`OS-POLICY-LIFECYCLE` remain `unverified` until these paths have functional
acceptance.
