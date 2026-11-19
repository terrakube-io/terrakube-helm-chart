# Configuration

## Node affinity, nodeSelector, taints and tolerations

The API, Registry, Executor and UI support using affinity, taints and tolerations. Use the following examples as reference:

### Example: API

```yaml
api:
  enabled: true
  replicaCount: "1"
  serviceType: "ClusterIP"
  resources:
    limits:
      cpu: 500m
      memory: 1024Mi
    requests:
      cpu: 200m
      memory: 256Mi
  tolerations: # OPTIONAL
  - key: "key1"
    operator: "Equal"
    value: "value1"
    effect: "NoSchedule"
  nodeSelector: # OPTIONAL
    disktype: ssd
  affinity:  # OPTIONAL
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: name
            operator: In
            values:
            - terrakube
  properties:
    databaseType: "POSTGRESQL"
```

## Custom CA certificates at runtime

Add custom CA certificates to the Terrakube components:

Example property ***security.caCerts***

```yaml
security:
  .....
  caCerts:
    terrakubeDemo1.pem: |
      -----BEGIN CERTIFICATE-----

      CERTIFICATE DATA

      -----END CERTIFICATE-----
    terrakubeDemo2.pem: |
      -----BEGIN CERTIFICATE-----

      CERTIFICATE DATA

      -----END CERTIFICATE-----
  ....
```

Terrakube components configuration with custom CA certificates:

```yaml
## API properties
api:
  enabled: true
  replicaCount: "1"
  serviceType: "ClusterIP"
  env:
  - name: SERVICE_BINDING_ROOT
    value: /mnt/platform/bindings
  volumes:
    - name: ca-certs
      secret:
        secretName: terrakube-ca-secrets
        items:
        - key: "terrakubeDemo1.pem"
          path: "terrakubeDemo1.pem"
        - key: "terrakubeDemo2.pem"
          path: "terrakubeDemo2.pem"
        - key: "type"
          path: "type"
  volumeMounts:
  - name: ca-certs
    mountPath: /mnt/platform/bindings/ca-certificates
    readOnly: true
  properties:
    databaseType: "POSTGRESQL"


## Executor properties
executor:
  enabled: true
  replicaCount: "1"
  serviceType: "ClusterIP"
  env:
  - name: SERVICE_BINDING_ROOT
    value: /mnt/platform/bindings
  volumes:
    - name: ca-certs
      secret:
        secretName: terrakube-ca-secrets
        items:
        - key: "terrakubeDemo1.pem"
          path: "terrakubeDemo1.pem"
        - key: "terrakubeDemo2.pem"
          path: "terrakubeDemo2.pem"
        - key: "type"
          path: "type"
  volumeMounts:
  - name: ca-certs
    mountPath: /mnt/platform/bindings/ca-certificates
    readOnly: true
  properties:
    toolsRepository: "https://github.com/terrakube-io/terrakube-extensions"
    toolsBranch: "main"

## Registry properties
registry:
  enabled: true
  replicaCount: "1"
  serviceType: "ClusterIP"
  env:
  - name: SERVICE_BINDING_ROOT
    value: /mnt/platform/bindings
  volumes:
    - name: ca-certs
      secret:
        secretName: terrakube-ca-secrets
        items:
        - key: "terrakubeDemo1.pem"
          path: "terrakubeDemo1.pem"
        - key: "terrakubeDemo2.pem"
          path: "terrakubeDemo2.pem"
        - key: "type"
          path: "type"
  volumeMounts:
  - name: ca-certs
    mountPath: /mnt/platform/bindings/ca-certificates
    readOnly: true
```

If the configuration is correct the pods log will show something like this:

```text
Added 2 additional CA certificate(s) to system truststore
```

## Enable OTEL for Terrakube components

To enable OTEL for Terrakube components use the following configuration example:

```yaml
## API properties
api:
  enabled: true
  replicaCount: "1"
  serviceType: "ClusterIP"
  otel:
    enabled: true
    metrics:
      port: 8081
      host:
    traces:
      type: jaeger # or zipkin
      endpoint: http://jaeger-collector:14268/api/traces


## Executor properties
executor:
  enabled: true
  replicaCount: "1"
  serviceType: "ClusterIP"
  otel:
    enabled: true
    metrics:
      port: 8081
      host:
    traces:
      type: zipkin # or jaeger
      endpoint: zipkin:9411/api/v2/spans

## Registry properties
registry:
  enabled: true
  replicaCount: "1"
  serviceType: "ClusterIP"
  otel:
    enabled: false # or true
    metrics:
      port: 8081
      host:
    traces:
      type: zipkin # or jaeger
      endpoint: zipkin:9411/api/v2/spans
```

## Dynamic credentials

To enable the feature use the following:

```yaml
api:
  dynamicCredentials:
    enabled: true
    publicKey: |
      -----BEGIN PUBLIC KEY-----
      REDACTED
      -----END PUBLIC KEY-----
    privateKey: |
      -----BEGIN PRIVATE KEY-----
      REDACTED
      -----END PRIVATE KEY-----
  volumes:
    - name: dynamic-credentials
      secret:
        secretName: terrakube-dynamic-credentials
        items:
          - key: public-key.pem
            path: public-key.pem
          - key: private-key.pem
            path: private-key.pem
  volumeMounts:
  - name: dynamic-credentials
    mountPath: /etc/terrakube/credentials
    readOnly: true
```

## Cloud CLI Tools (AWS, GCP, Azure)

The executor can be configured to install the AWS CLI, Google Cloud CLI (`gcloud`), and/or Azure CLI at pod startup — without building a custom image. Each tool is downloaded and installed by a dedicated init container into a shared volume that the executor container then mounts at `/opt/terrakube-tools`.

When any tool is enabled the chart automatically:
- Adds an init container that downloads and installs the tool
- Creates a shared `emptyDir` volume at `/opt/terrakube-tools`
- Mounts the volume into the executor container
- Prepends `/opt/terrakube-tools/bin` to `PATH`

### Enable all three CLIs

```yaml
executor:
  cliTools:
    awsCli:
      enabled: true
    gcloud:
      enabled: true
    azureCli:
      enabled: true
```

### Enable only AWS CLI

```yaml
executor:
  cliTools:
    awsCli:
      enabled: true
```

### Enable only Google Cloud CLI

```yaml
executor:
  cliTools:
    gcloud:
      enabled: true
```

### Enable only Azure CLI

```yaml
executor:
  cliTools:
    azureCli:
      enabled: true
```

### How each tool is installed

| Tool | Init container default image | Installation method |
|------|------------------------------|---------------------|
| AWS CLI v2 | `debian:bookworm-slim` | Official AWS CLI installer — self-contained bundle with bundled Python |
| Google Cloud CLI | `debian:bookworm-slim` | Official Linux `.tar.gz` archive — includes bundled Python |
| Azure CLI | `python:3.14-slim-bookworm` | Copies the image's Python (`/usr/local`) into the shared volume and runs `pip install azure-cli` there, because the executor image has no Python |

All three init container scripts detect the node architecture (`amd64` / `arm64`) at runtime and download the appropriate binary automatically.

### Overriding init container images

The `initImage` for each tool can be overridden. This is useful when the executor image uses a different libc variant (e.g., Alpine/musl instead of Debian/glibc).

> **Important:** The init container image must use the same libc variant (glibc or musl) as the executor image, with a glibc version no newer than the executor's, because the installed binaries and the copied Python are dynamically linked. The default executor image (`azbuilder/executor`) is Ubuntu 24.04 (glibc 2.39), and the default init images are Debian 12 bookworm (glibc 2.36), so the defaults work out of the box. The Azure CLI `initImage` must be an official `python` image, because the install copies Python from `/usr/local`.

```yaml
executor:
  cliTools:
    awsCli:
      enabled: true
      initImage: "debian:bookworm-slim"   # override for custom base OS
    gcloud:
      enabled: true
      initImage: "debian:bookworm-slim"
    azureCli:
      enabled: true
      initImage: "python:3.14-slim-bookworm"
```

### Using the CLIs in Terraform

Once enabled, the CLI tools are available to any Terraform `local-exec` provisioner or Terrakube extension script running inside the executor:

```hcl
resource "null_resource" "example" {
  provisioner "local-exec" {
    command = "aws s3 ls"
  }
}
```

Authentication credentials should be provided via Terraform workspace environment variables or a Kubernetes secret mounted into the executor.
