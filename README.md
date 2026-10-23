# Quarshell workspace

This repository contains cross-module tooling, engineering plans, OpenShell
compatibility inventories, and release/CI orchestration. Product code lives in
independent repositories. The workspace pins ten pre-stable Go modules in
[`tools/modules.lock`](tools/modules.lock); the TypeScript UI, Python SDK, and
MkDocs site are pinned separately in [`tools/projects.lock`](tools/projects.lock).

## Set up

```sh
git clone https://github.com/KLORQ/quarshell-workspace.git
cd quarshell-workspace
tools/checkout-modules.sh
tools/checkout-projects.sh
task workspace:sync
task workspace:check
task test
```

Use `task ci:pins` to compare workflow checkouts on all ten module `main`
branches with `tools/modules.lock`. Release tags keep the dependency versions
they were tested with; the check does not rewrite historical CI snapshots. The
[`plans/REMAINING.md`](plans/REMAINING.md) review tracks remaining OpenShell
parity and required acceptance evidence. The
[`plans/ARCHITECTURE-CONSOLIDATION.md`](plans/ARCHITECTURE-CONSOLIDATION.md)
documents module boundaries and the completed consolidation from twenty to ten
Go modules.

The [Quarshell user guide](https://klorq.github.io/sandbox.dev/) covers
installation and current product support.
