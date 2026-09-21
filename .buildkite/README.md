# Buildkite Setup

Create a Buildkite pipeline linked to `AXDOOMER/chromium` in the same cluster
used by `tgrep`. Buildkite checks out chromium using its GitHub integration;
`PIPELINE_SHALLOW_CLONE=true` keeps that checkout to a depth-one clone since
this repository is several gigabytes.

The generator detects no supported language marker at the repository root
(no `go.mod`, `Cargo.toml`, etc.) and falls back to a Dockerfile-only project,
overridden here by `Dockerfile.build`. That file does not use anything from
this checkout — it clones `depot_tools`, runs `fetch --nohooks --nohistory
chromium` to pull actual Chromium source directly from
`chromium.googlesource.com`, installs build dependencies, runs `gclient
runhooks`, and builds the `chrome` target with `gn`/`autoninja`. This mirrors
a build process already validated manually; the checked-out GitHub content
just triggers the pipeline.

Expect the build job to take hours; the `GITLAB_TOKEN` cluster secret and
`golang:1.25` bootstrap image are unrelated to the actual multi-hour Chromium
build, which instead runs entirely inside `Dockerfile.build`.
