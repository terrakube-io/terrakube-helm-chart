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
