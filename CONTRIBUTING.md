# Contributing

## Pull requests

1. Change the chart in `charts/terrakube/`.
2. Bump `version` in `charts/terrakube/Chart.yaml`. CI fails a chart change without a bump. See [RELEASING.md](RELEASING.md).
3. Add new options to both `values.yaml` and `values.schema.json`. Describe each option with a `# -- description` comment above it in `values.yaml`.
4. Regenerate the values reference: `docker run --rm -v "$PWD/charts:/helm-docs" jnorwood/helm-docs:v1.14.2` and commit `charts/terrakube/README.md`.
5. Keep defaults backward compatible.
6. Document upgrade actions in `docs/upgrading.md`.

## PR titles

Squash-merge uses the PR title as the commit message. Format: `type(scope): lowercase description`.

Types: `feat`, `fix`, `perf`, `revert`, `refactor`, `docs`, `test`, `build`, `ci`, `chore`. Append `!` for breaking changes.

## Local checks

```bash
for values in .github/ci-values/*.yaml; do
  helm lint charts/terrakube --strict --values "$values"
  helm template terrakube charts/terrakube --values "$values" \
    | kubeconform -strict -ignore-missing-schemas -summary
done
ct lint --config ct.yaml
```

Add a file to `.github/ci-values/` when a change needs a new scenario rendered in CI.
