# Gateway provider readiness acceptance — 2026-10-24

## Scope

`ReportProviderReadiness` now validates that an observation belongs to the active
sandbox runtime and currently connected supervisor instance. It checks the
durable supervisor lease including session ID, and compares policy hash and
provider environment revision with current Gateway state. The Gateway has no
authoritative desired-config revision source yet; it enforces monotonic config
revision reports within a supervisor session and persists that cursor.

The sandbox record persists the accepted session, sequence, and SHA-256 digest.
Older sequences, config revision rollback, and same-sequence reports with a changed body are rejected;
exact retries are accepted so a retry can finish provider binding state changes
after an interrupted request. Readiness acknowledgements require all installation
flags and an unspecified reason. Waiting reasons leave bindings unchanged;
credential, policy activation, and process installation failures fail the binding.

## Tests and verification

The acceptance tests cover a wrong supervisor session, initial acceptance,
exact retry, changed-body replay, zero and stale sequences, and next-sequence
acceptance. Existing RPC fixtures cover malformed readiness input. The provider
service also treats identical state transitions as no-ops, making retries stable.

Passed:

```sh
go test ./internal/application/sandbox ./internal/transport/applicationgrpc ./internal/application/provider
go test ./...
```

## Remaining scope

This verifies the local Gateway report path. Peer report forwarding, durable
cross-replica acceptance, and full attachment revision lifecycle still need
independent acceptance; `OS-PEER` remains `unverified`.
