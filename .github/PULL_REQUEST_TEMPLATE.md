<!-- The title becomes the commit message: type(scope): lowercase description. See CONTRIBUTING.md. -->

Closes #

## Changes

-

## Checks

- [ ] `Chart.yaml` `version` is not changed (the release PR bumps it)
- [ ] `values.yaml` and `values.schema.json` are updated together for new or changed options
- [ ] New options have a `# -- description` comment and `charts/terrakube/README.md` is regenerated with helm-docs
- [ ] Upgrade actions are in `docs/upgrading.md`
- [ ] `helm lint charts/terrakube` and `helm template` pass with the files in `.github/ci-values/`
- [ ] New options default to the previous behaviour, so existing installs upgrade unchanged

## Breaking change

- [ ] This changes defaults or renames/removes values. The title has `!` and the description says what users must change when upgrading.
