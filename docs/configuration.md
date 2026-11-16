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
