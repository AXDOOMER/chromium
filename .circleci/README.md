# CircleCI Setup

Followed on CircleCI and linked to `AXDOOMER/chromium`. Setup Workflows and
the `GITLAB_TOKEN` project environment variable are enabled/created via the
CircleCI API, same as for `linux`.

The generator finds no supported language marker at the repository root and
falls back to a Dockerfile-only project, overridden by `Dockerfile.build`
(same file used for the Buildkite pipeline). That file is self-contained: it
clones `depot_tools`, fetches actual Chromium source from
`chromium.googlesource.com`, installs build dependencies, and builds the
`chrome` target with `gn`/`autoninja`. The checked-out GitHub content is not
itself compiled — it only triggers the pipeline, mirroring an already
validated manual build.

`PIPELINE_SHALLOW_CLONE=true` is passed to the generator since this repo is
several gigabytes; the bootstrap job's own `checkout: {method: shallow}`
keeps that same clone shallow too.
