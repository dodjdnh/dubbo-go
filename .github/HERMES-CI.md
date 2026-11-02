# Personal Hermes CI control branch

This file and `.github/workflows/hermes-*.yml` are Fork-only infrastructure.
Do not merge these commits into branches contributed to Apache.

The workflow is loaded from the Fork default branch and accepts an immutable
commit SHA from a trusted personal branch. Only `dodjdnh` can invoke the
self-hosted job. No pull-request event is configured. The Windows `oss-ci`
command queues a globally serialized, rootless, ephemeral Runner on Hermes.

Keep product branches based on Apache upstream, not on this control branch.
Before an upstream PR, inspect the diff against upstream to ensure these
personal CI files are absent.

Profiles: `basic` retains the original core checks; `extended` adds generated
CLI project compilation and Linux RISC-V cross-build for Go, and RISC-V plus
the real Collector 0.112.0 OTTL contract for Admin. `codeql` runs the upstream
default Go query suite in a separate security-events-authorized job. `full`
runs extended and then CodeQL sequentially (Admin: extended only), against one
immutable source SHA. Each run uses the same global queue and resource cap.

Windows usage: `oss-ci dubbo-go feat/example -Profile full` or
`oss-ci dubbo-admin feat/example -Profile extended`. Add `-NoPush` to test the
current remote HEAD without pushing. Only Fork-default workflows are loaded;
feature branches need no personal CI files.

Samples integration, publishing and release automation remain out of scope.
Coverage generation is retained; Codecov upload is not configured. RISC-V is
cross-compilation, not emulated execution. Generated CLI projects retain the
SDK versions selected by upstream templates. The Collector binary is the
checksummed official native release, rather than extracted via host Docker.
CodeQL uses the official Go-only bundle, two threads and 3072MB; database
uploads are disabled, but SARIF upload and default security queries remain.
Code scanning results target the tested branch/SHA, not the control commit.
Analysis success does not imply zero security findings.

Required checks, including skipped checks, must succeed. Coverage and SARIF
are retained on Hermes (latest 20 runs, up to 14 days, within the 30GiB cache
budget). GitHub artifact mirroring is optional and may fail at account quota;
this does not hide core test or CodeQL upload failures.

The controller requires Windows to remain connected while obtaining the
Hermes slot and provisioning the short-lived Runner. Once started, the
server systemd job continues independently. No long-lived GitHub management
credential is stored on Hermes.
