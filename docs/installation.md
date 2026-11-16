# Installation

## Requirements

- Kubernetes 1.21+ and Helm 3
- An ingress controller (NGINX, AWS Load Balancer Controller or GKE) or a Gateway API implementation, see [ingress.md](ingress.md)
- An OIDC identity provider with group claims, connected through Dex, see [authentication.md](authentication.md)
- Object storage: AWS S3, Azure Storage Account, GCP bucket or MinIO, see [storage.md](storage.md)
- A database. PostgreSQL is the default. The `api.properties.databaseType` value is passed to the API, see the [Terrakube documentation](https://docs.terrakube.io/) for supported databases.

The chart bundles PostgreSQL, MinIO, Valkey and Dex for testing. Use external services in production.

## Install

```bash
helm repo add terrakube https://charts.terrakube.io
helm repo update
kubectl create namespace terrakube
helm install terrakube terrakube/terrakube -n terrakube -f values.yaml
```

List available versions with `helm search repo terrakube --versions`.

## Upgrade

```bash
helm repo update
helm upgrade terrakube terrakube/terrakube -n terrakube -f values.yaml
```

Read [upgrading.md](upgrading.md) before upgrading across a major version.

## Uninstall

```bash
helm uninstall terrakube -n terrakube
```

## Install from a local clone

```bash
git clone https://github.com/terrakube-io/terrakube-helm-chart.git
helm install terrakube ./terrakube-helm-chart/charts/terrakube -n terrakube --create-namespace -f values.yaml --dry-run --debug
helm install terrakube ./terrakube-helm-chart/charts/terrakube -n terrakube --create-namespace -f values.yaml
```

Remove `--dry-run --debug` from the second command to apply. The first command only renders and validates.

## Minikube

Follow the [Minikube + HTTPS guide](https://docs.terrakube.io/getting-started/deployment/minikube-+-https).

## Extra files (`otherFiles`)

Files in the `otherFiles` directory are added to the ConfigMap `terrakube-other-files`. Kubernetes limits a ConfigMap to 1MB.
