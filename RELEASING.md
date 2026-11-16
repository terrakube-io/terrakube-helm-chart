# Releasing

Published to:

- Helm repository: `https://terrakube-io.github.io/terrakube-helm-chart`
- GitHub releases: `terrakube-<version>`
- OCI registry: Docker Hub

## Process

1. PRs are squash-merged to `main` with conventional commit titles.
2. [`release-please.yml`](.github/workflows/release-please.yml) keeps a release PR open. It sets the next `version` in `charts/terrakube/Chart.yaml` and updates `charts/terrakube/CHANGELOG.md`.
3. Merge the release PR to publish. [`release.yml`](.github/workflows/release.yml) pushes the chart to the OCI registry, runs chart-releaser (GitHub release, tag and Helm repository index), and marks the release PR as tagged.

To retry a failed publish, run **Release Charts** from the Actions tab. Existing versions are skipped.

## Version bumps

| Commit title | Bump |
|---|---|
| `fix`, `perf` | patch |
| `feat` | minor |
| `type!:` or a `BREAKING CHANGE:` footer | major |
| `docs`, `test`, `build`, `ci`, `chore`, `refactor` | none |

`appVersion` in `Chart.yaml` is not changed by release-please. Update it in a `feat(chart)` or `fix(chart)` PR, which also releases.

## Required configuration

| Name | Type | Used for |
|---|---|---|
| `DOCKERHUB_USERNAME` | secret | OCI push |
| `DOCKERHUB_TOKEN` | secret | OCI push |
| `DOCKERHUB_NAMESPACE` | variable | OCI push |
| `RELEASE_PLEASE_TOKEN` | secret, optional | Runs CI on the release PR |

Without `RELEASE_PLEASE_TOKEN` the release PR is created with `GITHUB_TOKEN`, so CI does not run on it and required status checks stay pending.
