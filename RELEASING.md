# Releasing

Published to:

- Helm repository: `https://terrakube-io.github.io/terrakube-helm-chart`
- GitHub releases: `terrakube-<version>`
- OCI registry: Docker Hub

## Process

1. Bump `version` in `charts/terrakube/Chart.yaml` in the PR that changes the chart.
2. Merge to `main`. [`release.yml`](.github/workflows/release.yml) runs when `charts/**` changed.
3. The workflow pushes to the OCI registry and runs chart-releaser. Existing versions are skipped.

To retry a failed publish, run **Release Charts** from the Actions tab.

## Version bumps

| Change | Bump |
|---|---|
| Fix, docs, dependency patch, `appVersion` only | patch |
| New option, defaults unchanged | minor |
| Renamed or removed values, changed defaults | major |

## Required configuration

| Name | Type |
|---|---|
| `DOCKERHUB_USERNAME` | secret |
| `DOCKERHUB_TOKEN` | secret |
| `DOCKERHUB_NAMESPACE` | variable |
