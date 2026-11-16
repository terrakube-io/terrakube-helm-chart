# Terrakube Helm Chart

[![Release](https://img.shields.io/github/v/release/terrakube-io/terrakube-helm-chart?label=chart)](https://github.com/terrakube-io/terrakube-helm-chart/releases)
[![CI](https://github.com/terrakube-io/terrakube-helm-chart/actions/workflows/ci.yml/badge.svg)](https://github.com/terrakube-io/terrakube-helm-chart/actions/workflows/ci.yml)
[![License](https://img.shields.io/github/license/terrakube-io/terrakube-helm-chart)](LICENSE)

Helm chart for [Terrakube](https://github.com/terrakube-io/terrakube), an open-source Terraform management platform.

> The bundled PostgreSQL, MinIO and Valkey are for testing and development only. In production use an external PostgreSQL, Redis/Valkey, object storage and an external identity provider.

## Quick start

```bash
helm repo add terrakube https://charts.terrakube.io
helm repo update
kubectl create namespace terrakube
helm install terrakube terrakube/terrakube -n terrakube
```

Requires Kubernetes 1.21+ and Helm 3. For Minikube see the [Minikube + HTTPS guide](https://docs.terrakube.io/getting-started/deployment/minikube-+-https).

## Documentation

| | |
|---|---|
| [Installation](docs/installation.md) | Requirements, install, upgrade, uninstall |
| [Upgrading](docs/upgrading.md) | Changes that need action when upgrading |
| [Ingress](docs/ingress.md) | NGINX, AWS ALB, GKE, Gateway API |
| [Authentication](docs/authentication.md) | Dex connectors, OIDC endpoints, admin group |
| [Storage](docs/storage.md) | Azure, AWS S3, GCP, MinIO |
| [Configuration](docs/configuration.md) | Scheduling, CA certificates, OpenTelemetry, dynamic credentials |
| [Values reference](charts/terrakube/README.md#values) | Every value with type, default and description |
| [Examples](examples/README.md) | Complete values files per identity provider |

## Features

- **Ingress**: NGINX (generic), AWS ALB (work in progress), GKE and Gateway API
- **TLS/SSL Configuration**: Per-service TLS configuration with custom certificates
- **Google Cloud Integration**: GKE managed certificates, Cloud Armor, and BackendConfig support
- **AWS Integration**: ALB ingress with shared load balancer support
- **Authentication**: Dex OIDC integration with GitHub, Google, and other providers
- **Public OIDC/JWKS Endpoints**: Publicly accessible `.well-known/openid-configuration` and `.well-known/jwks` endpoints for OIDC discovery and JWT validation
- **Storage**: MinIO S3-compatible storage or external S3
- **Database**: PostgreSQL support with optional external database
- **Module Registry**: Private Terraform module registry
- **Workspace Management**: Terraform workspace execution and management

## Contributing

[CONTRIBUTING.md](CONTRIBUTING.md) · [RELEASING.md](RELEASING.md) · [SECURITY.md](SECURITY.md)
