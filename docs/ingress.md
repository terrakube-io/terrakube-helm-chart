# Ingress

Terrakube supports four ingress controllers: `generic` (nginx), `aws` (ALB), `gke` (Google Cloud Load Balancer), and `gatewayapi` (HttpRoute).

## Generic Ingress (Default)

Uses standard Kubernetes ingress with nginx controller:

```yaml
ingress:
  controller: "generic"
  ui:
    enabled: true
    domain: "terrakube-ui.example.com"
    ingressClassName: "nginx"
  api:
    enabled: true
    domain: "terrakube-api.example.com"
    ingressClassName: "nginx"
  registry:
    enabled: true
    domain: "terrakube-registry.example.com"
    ingressClassName: "nginx"
  executor:
    enabled: false
```

## AWS ALB Ingress (WIP)

Uses AWS Application Load Balancer for ingress:

```yaml
ingress:
  controller: "aws"
  ui:
    enabled: true
    hostname: "terrakube-ui.example.com"
  api:
    enabled: true
    hostname: "terrakube-api.example.com"
  registry:
    enabled: true
    hostname: "terrakube-registry.example.com"
  executor:
    enabled: false
```

## GKE Ingress

Uses Google Cloud Load Balancer for ingress:

```yaml
ingress:
  controller: "gke"
  ui:
    enabled: true
    hostname: "terrakube-ui.example.com"
  api:
    enabled: true
    hostname: "terrakube-api.example.com"
  registry:
    enabled: true
    hostname: "terrakube-registry.example.com"
  executor:
    enabled: false
```

## Gateway API (HTTPRoute)

```yaml
ingress:
  controller: "gatewayapi"
  gatewayapi:
    gateways:
    - name: "terrakube-gateway"
      namespace: "terrakube"
  ui:
    enabled: true
    domain: "terrakube-ui.example.com"
  api:
    enabled: true
    domain: "terrakube-api.example.com"
  registry:
    enabled: true
    domain: "terrakube-registry.example.com"
  executor:
    enabled: false
```

## Single-domain deployment (experimental)

Serve the UI, API, registry and Dex from one hostname, routed by path. The chart renders one
Ingress (or HTTPRoute with `controller: gatewayapi`) from `files/route-table.yaml` and skips the
per-component ingresses. Dex must use the shared host as its issuer:

```yaml
ingress:
  singleDomain:
    enabled: true
    domain: "terrakube.example.com"
    useTls: true
    tlsSecretName: tls-secret-terrakube
    ingressClassName: "nginx"
    maxBodySize: "512m"
dex:
  config:
    issuer: https://terrakube.example.com/dex
    # add https://terrakube.example.com to staticClients[].redirectURIs
```

`files/route-table.yaml` is a copy of `docs/single-domain/route-table.yaml` in
[terrakube](https://github.com/terrakube-io/terrakube); CI fails when the two differ. See the
[single-domain guide](https://docs.terrakube.io/getting-started/deployment/single-domain).

## Per-service security policies

You can apply Cloud Armor security policies to individual services:

```yaml
ingress:
  controller: "gke"
  gke:
    enabled: true
    backendConfig:
      create: true
      timeout: 30
      # Only restrict UI access - keep APIs open for external agents
      uiSecurityPolicy: "terrakube-ui-security-policy"
      # apiSecurityPolicy: ""          # Empty - external agents need access
      # registrySecurityPolicy: ""     # Empty - module downloads need access
```
