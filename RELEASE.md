# Release process

This document describes how to cut a new release of `action-busted`.

## Versioning

The action follows [Semantic Versioning](https://semver.org/):

- **Major** (`v2.0.0` → `v3.0.0`) — breaking changes (e.g. removed input, changed runtime).
- **Minor** (`v2.0.0` → `v2.1.0`) — new functionality, backwards-compatible.
- **Patch** (`v2.0.0` → `v2.0.1`) — bugfixes, image bumps without behavior changes.

Two tags are published per major release:

| Tag       | Mutability | Purpose                                                                   |
|-----------|------------|---------------------------------------------------------------------------|
| `vX.Y.Z`  | Immutable  | Pinned reference for users that want exact versions.                      |
| `vX`      | Moving     | Convenience tag pointing at the latest `vX.Y.Z`. Re-pointed on each release. |

## Image pinning

`action.yaml` references a fixed
[`docker-busted`](https://github.com/RagedUnicorn/docker-busted) image tag. GitHub Actions does
not allow `${{ inputs.* }}` interpolation in `runs.image`, so the busted version is locked per
action release.

Bumping busted (or the underlying Alpine base image) requires:

1. A new release in `docker-busted` published to GHCR.
2. Updating the `image:` line in `action.yaml`.
3. Cutting a new action release as described below.

## Cutting a release

GitHub Releases are created automatically by
[`.github/workflows/release.yml`](.github/workflows/release.yml) whenever a `vX.Y.Z` tag is
pushed. The moving `vX` tag does **not** trigger a release — only immutable semver tags do.

1. Land all changes on `master`.
2. Verify the working tree is clean and the local branch is up to date with `origin/master`.
3. Create the immutable version tag:
   ```bash
   git tag -a vX.Y.Z -m "vX.Y.Z - <short summary>"
   ```
4. For a new major release, create the moving major-version tag:
   ```bash
   git tag -a vX -m "vX latest"
   ```
   For a minor or patch release, re-point the existing major tag instead:
   ```bash
   git tag -fa vX -m "vX latest"
   ```
5. Push the tags:
   ```bash
   # New major release
   git push origin vX.Y.Z vX

   # Minor or patch release (force-push the moving tag only)
   git push origin vX.Y.Z
   git push origin vX --force
   ```
6. The release workflow runs against the pushed `vX.Y.Z` tag and produces a GitHub Release with
   an auto-generated changelog. For majors with breaking changes, edit the release body in the
   GitHub UI to add a **Breaking changes** section and the pinned `docker-busted` image tag.
