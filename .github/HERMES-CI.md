# Personal Hermes CI control branch

This file and `.github/workflows/hermes-ci.yml` are Fork-only infrastructure.
Do not merge these commits into branches contributed to Apache.

The workflow is loaded from the Fork default branch and accepts an immutable
commit SHA from a trusted personal branch. Only `dodjdnh` can invoke the
self-hosted job. No pull-request event is configured. The Windows `oss-ci`
command queues a globally serialized, rootless, ephemeral Runner on Hermes.

Keep product branches based on Apache upstream, not on this control branch.
Before an upstream PR, inspect the diff against upstream to ensure these
personal CI files are absent.

Phase 1 does not execute Samples integration, generated CLI project tests,
RISC-V, release tasks, or CodeQL. Coverage generation is retained; Codecov
upload is not configured. A skipped required check is not counted as a pass.

The controller requires Windows to remain connected while obtaining the
Hermes slot and provisioning the short-lived Runner. Once started, the
server systemd job continues independently. No long-lived GitHub management
credential is stored on Hermes.
