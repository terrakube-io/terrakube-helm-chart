# terrakube

![Version: 4.7.19](https://img.shields.io/badge/Version-4.7.19-informational?style=flat-square) ![AppVersion: 2.34.1](https://img.shields.io/badge/AppVersion-2.34.1-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square)

Deploys [Terrakube](https://github.com/terrakube-io/terrakube) on Kubernetes.

- [Installation and configuration guides](https://github.com/terrakube-io/terrakube-helm-chart/tree/main/docs)
- [Example values](https://github.com/terrakube-io/terrakube-helm-chart/tree/main/examples)

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://charts.bitnami.com/bitnami | minio | 17.0.21 |
| https://charts.bitnami.com/bitnami | postgresql | 16.7.27 |
| https://charts.dexidp.io | dex | 0.24.0 |
| https://valkey.io/valkey-helm/ | valkey | 0.9.4 |

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| api | object | `{"automountServiceAccountToken":true,"cache":{"moduleCacheMaxIdle":"128","moduleCacheMaxTotal":"128","moduleCacheMinIdle":"64","moduleCacheSchedule":"0 */3 * ? * *","moduleCacheTimeout":"600000","releaseCacheExpirationMinutes":"30"},"configMaps":["terrakube-api-config"],"containerSecurityContext":{},"defaultDatabase":true,"defaultRedis":true,"dynamicCredentials":{"enabled":false,"privateKey":"","privateKeyPath":"/etc/terrakube/credentials/private-key.pem","publicKey":"","publicKeyPath":"/etc/terrakube/credentials/public-key.pem"},"enabled":true,"ephemeralExecution":{"activeDeadlineSeconds":"","backoffLimit":"","configMapName":"","emptyDirSecret":true,"enabled":false,"initSecretImage":"","podAnnotations":{},"secretName":["terrakube-executor-secrets"],"terminationGracePeriodSeconds":"","ttlSecondsAfterFinished":""},"existingSecret":false,"image":"azbuilder/api-server","imagePullSecrets":[],"initContainers":[],"livenessProbe":{"periodSeconds":10},"loadSampleData":true,"otel":{"enabled":false,"metrics":{"host":"0.0.0.0","port":"9464"},"traces":{"endpoint":"http://terrakube-api.minikube.net:9411/api/v2/spans","type":""}},"podAnnotations":{},"podDisruptionBudget":{"enabled":false,"minAvailable":1},"podLabels":{},"properties":{"databaseHostname":"","databaseName":"","databasePassword":"","databasePort":"5432","databaseSchema":"public","databaseSslMode":"disable","databaseType":"POSTGRESQL","databaseUser":"","executorUrl":"http://terrakube-executor-service:8090","redisHostname":"","redisPassword":"","redisPort":"6379","stateMaxSizeBytes":"104857600"},"rbac":{"create":false,"roleBindingName":"terrakube-api-role-binding","roleName":"terrakube-api-role"},"readinessProbe":{"periodSeconds":10},"replicaCount":"1","resources":{},"revisionHistoryLimit":3,"secrets":["terrakube-api-secrets"],"securityContext":{},"serviceAccountAnnotations":{},"serviceAccountName":"","serviceAnnotations":{},"serviceType":"ClusterIP","startupProbe":{"failureThreshold":30,"initialDelaySeconds":30,"periodSeconds":5},"terraformReleasesUrl":"https://releases.hashicorp.com/terraform/index.json","tofuReleasesUrl":"https://api.github.com/repos/opentofu/opentofu/releases","version":""}` | API properties |
| api.automountServiceAccountToken | bool | `true` | Mount the ServiceAccount token into the pods |
| api.cache.releaseCacheExpirationMinutes | string | `"30"` | Minutes the terraform/tofu release lists are cached in Redis Every expiration triggers one outbound call per API replica. |
| api.configMaps | list | `["terrakube-api-config"]` | ConfigMaps loaded into the container environment |
| api.containerSecurityContext | object | `{}` | Container security context for API |
| api.defaultDatabase | bool | `true` | Enable default database using postgresql helm chart |
| api.defaultRedis | bool | `true` | Enable default Valkey using the official Valkey helm chart |
| api.dynamicCredentials.enabled | bool | `false` | This will enable dyanmic credentials secret |
| api.dynamicCredentials.privateKey | string | `""` | Private key for dynamic credentials |
| api.dynamicCredentials.privateKeyPath | string | `"/etc/terrakube/credentials/private-key.pem"` | Default value /etc/terrakube/credentials/private-key.pem |
| api.dynamicCredentials.publicKey | string | `""` | Public key for dynamic credentials |
| api.dynamicCredentials.publicKeyPath | string | `"/etc/terrakube/credentials/public-key.pem"` | Default value /etc/terrakube/credentials/public-key.pem |
| api.enabled | bool | `true` | Enable API component deployment |
| api.ephemeralExecution.activeDeadlineSeconds | string | `""` | Seconds the ephemeral job is allowed to run before Kubernetes terminates it; unset means unbounded (today's behaviour) |
| api.ephemeralExecution.backoffLimit | string | `""` | Number of retries before the ephemeral job is marked failed; unset means Kubernetes' own default (6). Set to "0" for at-most-once execution |
| api.ephemeralExecution.emptyDirSecret | bool | `true` | Controls whether ephemeral job mounts secrets via emptyDir initContainer (defaults to executor.emptyDirSecret) |
| api.ephemeralExecution.initSecretImage | string | `""` | Init image used when emptyDirSecret is true (defaults to executor.initSecretImage) |
| api.ephemeralExecution.terminationGracePeriodSeconds | string | `""` | Seconds the ephemeral pod gets to shut down cleanly after SIGTERM before Kubernetes SIGKILLs it; unset means 60 |
| api.ephemeralExecution.ttlSecondsAfterFinished | string | `""` | Seconds after completion before the ephemeral job is automatically deleted; unset means 30 |
| api.existingSecret | bool | `false` | Use an existing Secret instead of the one the chart creates |
| api.image | string | `"azbuilder/api-server"` | API image repository |
| api.imagePullSecrets | list | `[]` | Specific Secret used to pull images from private repository |
| api.initContainers | list | `[]` | Init containers for API deployment |
| api.livenessProbe | object | `{"periodSeconds":10}` | Liveness probe settings |
| api.otel.enabled | bool | `false` | Enable OpenTelemetry |
| api.otel.traces.endpoint | string | `"http://terrakube-api.minikube.net:9411/api/v2/spans"` | endpoint its just example, change it to your jaeger endpoint |
| api.podAnnotations | object | `{}` | Extra annotations for the pods |
| api.podDisruptionBudget.enabled | bool | `false` | Create a PodDisruptionBudget |
| api.podDisruptionBudget.minAvailable | int | `1` | Minimum available pods, integer or percentage. Mutually exclusive with maxUnavailable |
| api.podLabels | object | `{}` | Extra labels for the pods |
| api.properties.databaseHostname | string | `""` | External database hostname |
| api.properties.databaseName | string | `""` | External database name |
| api.properties.databasePassword | string | `""` | External database password |
| api.properties.databaseType | string | `"POSTGRESQL"` | Database type: H2/SQL_AZURE/POSTGRESQL/MYSQL |
| api.properties.databaseUser | string | `""` | External database username |
| api.properties.stateMaxSizeBytes | string | `"104857600"` | Maximum state file size accepted by the Terraform HTTP backend endpoint in bytes (default: 104857600 = 100MB) |
| api.readinessProbe | object | `{"periodSeconds":10}` | Readiness probe settings |
| api.replicaCount | string | `"1"` | Number of API pod replicas |
| api.resources | object | `{}` | Container resource requests and limits |
| api.revisionHistoryLimit | int | `3` | Number of old ReplicaSets to keep |
| api.secrets | list | `["terrakube-api-secrets"]` | Secrets loaded into the container environment |
| api.securityContext | object | `{}` | Pod security context for API |
| api.serviceAccountAnnotations | object | `{}` | Annotations for the ServiceAccount |
| api.serviceAccountName | string | `""` | Kubernetes Service Account name |
| api.serviceAnnotations | object | `{}` | Annotations for the Service |
| api.serviceType | string | `"ClusterIP"` | Kubernetes service type (ClusterIP/NodePort/LoadBalancer) |
| api.startupProbe | object | `{"failureThreshold":30,"initialDelaySeconds":30,"periodSeconds":5}` | Startup probe settings |
| api.version | string | `""` | Terrakube API version |
| dex | object | `{"config":{"connectors":[{"config":{"bindDN":"cn=admin,dc=example,dc=org","bindPW":"admin","groupSearch":{"baseDN":"ou=Groups,dc=example,dc=org","filter":"(objectClass=groupOfNames)","nameAttr":"cn","userMatchers":[{"groupAttr":"member","userAttr":"DN"}]},"host":"terrakube-openldap-service:1389","insecureNoSSL":true,"userSearch":{"baseDN":"ou=users,dc=example,dc=org","emailAttr":"mail","filter":"(objectClass=person)","idAttr":"DN","nameAttr":"cn","username":"mail"},"usernamePrompt":"Email Address"},"id":"ldap","name":"OpenLDAP","type":"ldap"}],"issuer":"http://terrakube-api.minikube.net/dex","oauth2":{"responseTypes":["code","token","id_token"],"skipApprovalScreen":true},"staticClients":[{"id":"example-app","name":"example-app","public":true,"redirectURIs":["http://terrakube-ui.minikube.net","/device/callback","http://localhost:10000/login","http://localhost:10001/login"]}],"storage":{"type":"memory"},"web":{"allowedOrigins":["*"],"http":"0.0.0.0:5556"}},"enabled":true,"existingSecret":false,"revisionHistoryLimit":3,"volumeMount":[{"mountPath":"/etc/gcp/secret","name":"gcp-credentials","readOnly":true}],"volumes":[{"name":"gcp-credentials","secret":{"items":[{"key":"gcp-credentials","path":"gcp-credentials"}],"secretName":"terrakube-dex-secrets"}}]}` | Dex |
| dex.config.connectors[0].config.bindDN | string | `"cn=admin,dc=example,dc=org"` | This would normally be a read-only user |
| dex.config.connectors[0].config.groupSearch.nameAttr | string | `"cn"` | The group name should be the "cn" value |
| dex.config.connectors[0].config.host | string | `"terrakube-openldap-service:1389"` | The following configurations seem to work with OpenLDAP:  1) Plain LDAP, without TLS: |
| dex.config.connectors[0].config.userSearch.idAttr | string | `"DN"` | "DN" (case sensitive) is a special attribute name. It indicates that this value should be taken from the entity's DN not an attribute on the entity. |
| dex.enabled | bool | `true` | Enable Dex OIDC authentication component |
| enableIPv6 | bool | `true` |  |
| executor | object | `{"apiServiceUrl":"http://terrakube-api-service:8080","configMaps":["terrakube-executor-config"],"containerSecurityContext":{},"emptyDirSecret":true,"enabled":true,"existingSecret":false,"image":"azbuilder/executor","imagePullSecrets":[],"initContainers":[],"initRestartPolicy":"Always","initSecretImage":"busybox:1.37.0","livenessProbe":{"periodSeconds":10},"otel":{"enabled":false,"metrics":{"host":"0.0.0.0","port":"9464"},"traces":{"endpoint":"http://terrakube-api.minikube.net:9411/api/v2/spans","type":"jaeger"}},"podAnnotations":{},"podDisruptionBudget":{"enabled":false,"minAvailable":1},"podLabels":{},"properties":{"httpBackend":true,"maxJobDurationMinutes":"360","toolsBranch":"main","toolsRepository":"https://github.com/terrakube-io/terrakube-extensions"},"readinessProbe":{"failureThreshold":1,"periodSeconds":2},"replicaCount":"1","resources":{},"revisionHistoryLimit":3,"secrets":["terrakube-executor-secrets"],"securityContext":{"fsGroup":1000},"serviceAccountName":"","serviceType":"ClusterIP","startupProbe":{"failureThreshold":30,"periodSeconds":5},"version":""}` | Executor properties |
| executor.configMaps | list | `["terrakube-executor-config"]` | ConfigMaps loaded into the container environment |
| executor.containerSecurityContext | object | `{}` | Fill securityContext field in the container spec |
| executor.enabled | bool | `true` | Enable Executor component deployment |
| executor.existingSecret | bool | `false` | Use an existing Secret instead of the one the chart creates |
| executor.image | string | `"azbuilder/executor"` | Executor image repository |
| executor.imagePullSecrets | list | `[]` | Specific Secret used to pull images from private repository |
| executor.initContainers | list | `[]` | Init containers for executor deployment |
| executor.livenessProbe | object | `{"periodSeconds":10}` | Liveness probe settings |
| executor.otel.enabled | bool | `false` | Enable OpenTelemetry |
| executor.otel.traces.endpoint | string | `"http://terrakube-api.minikube.net:9411/api/v2/spans"` | endpoint its just example, change it to your jaeger endpoint |
| executor.podAnnotations | object | `{}` | Extra annotations for the pods |
| executor.podDisruptionBudget.enabled | bool | `false` | Create a PodDisruptionBudget |
| executor.podDisruptionBudget.minAvailable | int | `1` | Minimum available pods, integer or percentage. Mutually exclusive with maxUnavailable |
| executor.podLabels | object | `{}` | Extra labels for the pods |
| executor.properties.httpBackend | bool | `true` | Enable Terraform HTTP state backend (requires Terrakube 2.34.0+) When enabled, Terrakube executor generates a terraform HTTP backend override pointing to Terrakube API, and passes scoped workspace access tokens for state synchronization and locking. |
| executor.properties.maxJobDurationMinutes | string | `"360"` | Longest a single job may run before this pod is marked unhealthy (liveness) so Kubernetes restarts it - covers a hung terraform process or infinite-looping hook script. |
| executor.properties.toolsBranch | string | `"main"` | Example: main |
| executor.properties.toolsRepository | string | `"https://github.com/terrakube-io/terrakube-extensions"` | Example: https://github.com/terrakube-io/terrakube-extensions |
| executor.readinessProbe | object | `{"failureThreshold":1,"periodSeconds":2}` | Readiness probe settings |
| executor.readinessProbe.periodSeconds | int | `2` | Low periodSeconds/failureThreshold so a pod that just started a job is pulled out of the Service's endpoints almost immediately, instead of the Kubernetes default of 3 consecutive failures (~2 extra probe cycles of routing new jobs to a busy pod). |
| executor.replicaCount | string | `"1"` | Number of Executor pod replicas |
| executor.resources | object | `{}` | Container resource requests and limits |
| executor.revisionHistoryLimit | int | `3` | Number of old ReplicaSets to keep |
| executor.secrets | list | `["terrakube-executor-secrets"]` | Secrets loaded into the container environment |
| executor.securityContext | object | `{"fsGroup":1000}` | Fill securityContext field |
| executor.serviceAccountName | string | `""` | Kubernetes Service Account name |
| executor.serviceType | string | `"ClusterIP"` | ClusterIP/NodePort/LoadBalancer/ExternalName |
| executor.startupProbe | object | `{"failureThreshold":30,"periodSeconds":5}` | Startup probe settings |
| executor.version | string | `""` | Terrakube Executor version |
| global.imagePullSecrets | list | `[]` | Global Secret used to pull images from private repository |
| ingress | object | `{"api":{"annotations":{"nginx.ingress.kubernetes.io/proxy-set-headers":"terrakube/custom-headers","nginx.ingress.kubernetes.io/use-regex":"true"},"domain":"terrakube-api.minikube.net","enabled":true,"ingressClassName":"nginx","path":"/","pathType":"Prefix","tlsSecretName":"tls-secret-api-terrakube","useTls":false},"aws":{"annotations":{"alb.ingress.kubernetes.io/listen-ports":"[{\"HTTPS\":443}, {\"HTTP\":80}]","alb.ingress.kubernetes.io/scheme":"internet-facing","alb.ingress.kubernetes.io/ssl-redirect":"443","alb.ingress.kubernetes.io/target-type":"ip","kubernetes.io/ingress.class":"alb"},"certificateArn":"","enabled":false,"securityGroups":[],"sharedLoadBalancer":{"enabled":false,"groupName":""},"subnets":[],"targetGroupBinding":{"apiTargetGroupARN":"","create":false,"registryTargetGroupARN":"","uiTargetGroupARN":""}},"controller":"generic","dex":{"annotations":{"nginx.ingress.kubernetes.io/proxy-set-headers":"terrakube/custom-headers","nginx.ingress.kubernetes.io/use-regex":"true"},"enabled":true,"path":"/dex/","pathType":"Prefix"},"executor":{"annotations":{"nginx.ingress.kubernetes.io/use-regex":"true"},"domain":"terrakube-executor.minikube.net","enabled":false,"ingressClassName":"nginx","path":"/","pathType":"Prefix","tlsSecretName":"tls-secret-executor-terrakube"},"gatewayapi":{"annotations":{},"apiVersion":"v1beta1","enabled":false,"gateways":[]},"gke":{"annotations":{"kubernetes.io/ingress.class":"gce"},"apiStaticIPName":"","backendConfig":{"apiSecurityPolicy":"","create":false,"executorSecurityPolicy":"","registrySecurityPolicy":"","timeout":30,"uiSecurityPolicy":""},"enabled":false,"executorStaticIPName":"","externalDNS":{"perIngressProxy":{"api":false,"dex":false,"executor":false,"registry":false,"ui":false}},"frontendConfig":{"apiSslPolicy":"","create":false,"executorSslPolicy":"","redirectToHttps":true,"registrySslPolicy":"","uiSslPolicy":""},"managedCertificate":{"create":false},"registryStaticIPName":"","uiStaticIPName":""},"includeTlsHosts":true,"registry":{"annotations":{"nginx.ingress.kubernetes.io/proxy-set-headers":"terrakube/custom-headers","nginx.ingress.kubernetes.io/use-regex":"true"},"domain":"terrakube-reg.minikube.net","enabled":true,"ingressClassName":"nginx","path":"/","pathType":"Prefix","tlsSecretName":"tls-secret-reg-terrakube","useTls":false},"ui":{"annotations":{"nginx.ingress.kubernetes.io/use-regex":"true"},"domain":"terrakube-ui.minikube.net","enabled":true,"ingressClassName":"nginx","path":"/","pathType":"Prefix","tlsSecretName":"tls-secret-ui-terrakube","useTls":true},"useTls":false}` | Ingress properties |
| ingress.api.annotations | object | `{"nginx.ingress.kubernetes.io/proxy-set-headers":"terrakube/custom-headers","nginx.ingress.kubernetes.io/use-regex":"true"}` | Custom annotations for API ingress |
| ingress.api.domain | string | `"terrakube-api.minikube.net"` | Domain name for API service |
| ingress.api.enabled | bool | `true` | Enable API ingress (default: true) |
| ingress.api.ingressClassName | string | `"nginx"` | Ingress class name (e.g., "nginx", "gce") |
| ingress.api.path | string | `"/"` | URL path for API service (default: "/") |
| ingress.api.pathType | string | `"Prefix"` | Path matching type: "Prefix", "Exact", or "ImplementationSpecific" |
| ingress.api.tlsSecretName | string | `"tls-secret-api-terrakube"` | Kubernetes TLS secret name for API certificates |
| ingress.api.useTls | bool | `false` | Enable TLS/SSL for API (default: false) |
| ingress.aws | object | `{"annotations":{"alb.ingress.kubernetes.io/listen-ports":"[{\"HTTPS\":443}, {\"HTTP\":80}]","alb.ingress.kubernetes.io/scheme":"internet-facing","alb.ingress.kubernetes.io/ssl-redirect":"443","alb.ingress.kubernetes.io/target-type":"ip","kubernetes.io/ingress.class":"alb"},"certificateArn":"","enabled":false,"securityGroups":[],"sharedLoadBalancer":{"enabled":false,"groupName":""},"subnets":[],"targetGroupBinding":{"apiTargetGroupARN":"","create":false,"registryTargetGroupARN":"","uiTargetGroupARN":""}}` | AWS specific configuration |
| ingress.aws.annotations | object | `{"alb.ingress.kubernetes.io/listen-ports":"[{\"HTTPS\":443}, {\"HTTP\":80}]","alb.ingress.kubernetes.io/scheme":"internet-facing","alb.ingress.kubernetes.io/ssl-redirect":"443","alb.ingress.kubernetes.io/target-type":"ip","kubernetes.io/ingress.class":"alb"}` | Global AWS ALB annotations applied to all ingresses |
| ingress.aws.certificateArn | string | `""` | AWS ACM certificate ARN for SSL termination |
| ingress.aws.enabled | bool | `false` | Enable AWS ALB ingress features (default: false) |
| ingress.aws.sharedLoadBalancer.enabled | bool | `false` | Use shared ALB across multiple ingresses (default: false) |
| ingress.aws.sharedLoadBalancer.groupName | string | `""` | ALB group name for shared load balancer |
| ingress.controller | string | `"generic"` | Ingress controller type: "generic", "aws", or "gke" and "gatewayapi" |
| ingress.dex.annotations | object | `{"nginx.ingress.kubernetes.io/proxy-set-headers":"terrakube/custom-headers","nginx.ingress.kubernetes.io/use-regex":"true"}` | Ingress annotations |
| ingress.dex.path | string | `"/dex/"` | URL path for Dex OIDC service (default: "/dex") |
| ingress.dex.pathType | string | `"Prefix"` | ImplementationSpecific/Exact/Prefix |
| ingress.executor.annotations | object | `{"nginx.ingress.kubernetes.io/use-regex":"true"}` | Custom annotations for Executor ingress |
| ingress.executor.domain | string | `"terrakube-executor.minikube.net"` | Domain name for Executor service |
| ingress.executor.enabled | bool | `false` | Enable Executor ingress (default: false, usually internal-only) |
| ingress.executor.ingressClassName | string | `"nginx"` | Ingress class name (e.g., "nginx", "gce") |
| ingress.executor.path | string | `"/"` | URL path for Executor service (default: "/") |
| ingress.executor.pathType | string | `"Prefix"` | Path matching type: "Prefix", "Exact", or "ImplementationSpecific" |
| ingress.executor.tlsSecretName | string | `"tls-secret-executor-terrakube"` | Kubernetes TLS secret name for Executor certificates |
| ingress.gatewayapi.enabled | bool | `false` | Enable Gateway API features (default: false) |
| ingress.gatewayapi.gateways | list | `[]` | Gateway API gateway references |
| ingress.gke | object | `{"annotations":{"kubernetes.io/ingress.class":"gce"},"apiStaticIPName":"","backendConfig":{"apiSecurityPolicy":"","create":false,"executorSecurityPolicy":"","registrySecurityPolicy":"","timeout":30,"uiSecurityPolicy":""},"enabled":false,"executorStaticIPName":"","externalDNS":{"perIngressProxy":{"api":false,"dex":false,"executor":false,"registry":false,"ui":false}},"frontendConfig":{"apiSslPolicy":"","create":false,"executorSslPolicy":"","redirectToHttps":true,"registrySslPolicy":"","uiSslPolicy":""},"managedCertificate":{"create":false},"registryStaticIPName":"","uiStaticIPName":""}` | GKE specific configuration |
| ingress.gke.annotations | object | `{"kubernetes.io/ingress.class":"gce"}` | Global GKE annotations applied to all ingresses |
| ingress.gke.apiStaticIPName | string | `""` | GCP static IP name for API service |
| ingress.gke.backendConfig.apiSecurityPolicy | string | `""` | Cloud Armor security policy for API service (usually empty) |
| ingress.gke.backendConfig.create | bool | `false` | Create BackendConfig resources for health checks (default: false) |
| ingress.gke.backendConfig.executorSecurityPolicy | string | `""` | Cloud Armor security policy for Executor service (usually empty) |
| ingress.gke.backendConfig.registrySecurityPolicy | string | `""` | Cloud Armor security policy for Registry service (usually empty) |
| ingress.gke.backendConfig.timeout | int | `30` | Backend timeout in seconds (default: 30) |
| ingress.gke.backendConfig.uiSecurityPolicy | string | `""` | Cloud Armor security policy for UI service (restrict user access) |
| ingress.gke.enabled | bool | `false` | Enable GKE ingress features (default: false) |
| ingress.gke.executorStaticIPName | string | `""` | GCP static IP name for Executor service |
| ingress.gke.externalDNS.perIngressProxy | object | `{"api":false,"dex":false,"executor":false,"registry":false,"ui":false}` | Per-service Cloudflare proxy settings |
| ingress.gke.frontendConfig.apiSslPolicy | string | `""` | SSL policy for API service |
| ingress.gke.frontendConfig.create | bool | `false` | Create FrontendConfig resources (default: false) |
| ingress.gke.frontendConfig.executorSslPolicy | string | `""` | SSL policy for Executor service |
| ingress.gke.frontendConfig.registrySslPolicy | string | `""` | SSL policy for Registry service |
| ingress.gke.frontendConfig.uiSslPolicy | string | `""` | SSL policy for UI service |
| ingress.gke.managedCertificate.create | bool | `false` | Create GCP managed SSL certificates (default: false) |
| ingress.gke.registryStaticIPName | string | `""` | GCP static IP name for Registry service |
| ingress.gke.uiStaticIPName | string | `""` | GCP static IP name for UI service |
| ingress.includeTlsHosts | bool | `true` | Include TLS hosts in ingress configuration (default: true) |
| ingress.registry.annotations | object | `{"nginx.ingress.kubernetes.io/proxy-set-headers":"terrakube/custom-headers","nginx.ingress.kubernetes.io/use-regex":"true"}` | Custom annotations for Registry ingress |
| ingress.registry.domain | string | `"terrakube-reg.minikube.net"` | Domain name for Registry service |
| ingress.registry.enabled | bool | `true` | Enable Registry ingress (default: true) |
| ingress.registry.ingressClassName | string | `"nginx"` | Ingress class name (e.g., "nginx", "gce") |
| ingress.registry.path | string | `"/"` | URL path for Registry service (default: "/") |
| ingress.registry.pathType | string | `"Prefix"` | Path matching type: "Prefix", "Exact", or "ImplementationSpecific" |
| ingress.registry.tlsSecretName | string | `"tls-secret-reg-terrakube"` | Kubernetes TLS secret name for Registry certificates |
| ingress.registry.useTls | bool | `false` | Enable TLS/SSL for Registry (default: false) |
| ingress.ui.annotations | object | `{"nginx.ingress.kubernetes.io/use-regex":"true"}` | Custom annotations for UI ingress |
| ingress.ui.domain | string | `"terrakube-ui.minikube.net"` | Domain name for UI service |
| ingress.ui.enabled | bool | `true` | Enable UI ingress (default: true) |
| ingress.ui.ingressClassName | string | `"nginx"` | Ingress class name (e.g., "nginx", "gce") |
| ingress.ui.path | string | `"/"` | URL path for UI service (default: "/") |
| ingress.ui.pathType | string | `"Prefix"` | Path matching type: "Prefix", "Exact", or "ImplementationSpecific" |
| ingress.ui.tlsSecretName | string | `"tls-secret-ui-terrakube"` | Kubernetes TLS secret name for UI certificates |
| ingress.ui.useTls | bool | `true` | Enable TLS/SSL for UI (default: false) |
| minio | object | `{"auth":{"rootPassword":"superadmin","rootUser":"admin"},"clientImage":{"repository":"bitnamilegacy/minio-client","tag":"2025.7.21-debian-12-r2"},"console":{"enabled":false},"defaultBuckets":"terrakube","image":{"repository":"bitnamilegacy/minio","tag":"2025.7.23-debian-12-r3"}}` | Default Storage Minio Configuration |
| name | string | `"terrakube"` | Use "Terrakube" |
| openldap | object | `{"adminPass":"admin","adminUser":"admin","baseRoot":"dc=example,dc=org","containerSecurityContext":{},"image":"bitnamilegacy/openldap","imagePullSecrets":[],"initContainers":[],"podAnnotations":{},"podLabels":{},"revisionHistoryLimit":3,"securityContext":{},"version":"latest"}` | OpenLdap |
| openldap.adminPass | string | `"admin"` | LDAP deployment admin password |
| openldap.adminUser | string | `"admin"` | LDAP deployment admin user |
| openldap.baseRoot | string | `"dc=example,dc=org"` | LDAP baseDN (or suffix) of the LDAP tree |
| openldap.containerSecurityContext | object | `{}` | Container security context for LDAP deployment |
| openldap.image | string | `"bitnamilegacy/openldap"` | LDAP deployment image repository |
| openldap.imagePullSecrets | list | `[]` | Secret used to pull images from private repository |
| openldap.initContainers | list | `[]` | Init containers for LDAP deployment |
| openldap.podLabels | object | `{}` | Pod labels for LDAP deployment |
| openldap.securityContext | object | `{}` | Security context for LDAP deployment |
| openldap.version | string | `"latest"` | LDAP deployment image tag |
| postgresql | object | `{"auth":{"database":"terrakube","password":"terrakubepassword","username":"terrakube"},"image":{"repository":"bitnamilegacy/postgresql","tag":"17.6.0-debian-12-r4"}}` | Default Database Postgresql |
| registry | object | `{"configMaps":["terrakube-registry-config"],"containerSecurityContext":{},"enabled":true,"existingSecret":false,"image":"azbuilder/open-registry","imagePullSecrets":[],"initContainers":[],"livenessProbe":{"periodSeconds":10},"otel":{"enabled":false,"metrics":{"host":"0.0.0.0","port":"9464"},"traces":{"endpoint":"http://terrakube-api.minikube.net:9411/api/v2/spans","type":"jaeger"}},"podAnnotations":{},"podDisruptionBudget":{"enabled":false,"minAvailable":1},"podLabels":{},"readinessProbe":{"periodSeconds":10},"replicaCount":"1","resources":{},"revisionHistoryLimit":3,"secrets":["terrakube-registry-secrets"],"securityContext":{},"serviceAccountName":"","serviceAnnotations":{},"serviceType":"ClusterIP","startupProbe":{"failureThreshold":30,"periodSeconds":5},"version":""}` | Registry properties |
| registry.configMaps | list | `["terrakube-registry-config"]` | ConfigMaps loaded into the container environment |
| registry.containerSecurityContext | object | `{}` | Container security context for Registry |
| registry.enabled | bool | `true` | Enable Registry component deployment |
| registry.existingSecret | bool | `false` | Use an existing Secret instead of the one the chart creates |
| registry.image | string | `"azbuilder/open-registry"` | Registry image repository |
| registry.imagePullSecrets | list | `[]` | Specific Secret used to pull images from private repository |
| registry.initContainers | list | `[]` | Init containers for registry deployment |
| registry.livenessProbe | object | `{"periodSeconds":10}` | Liveness probe settings |
| registry.otel.enabled | bool | `false` | Enable OpenTelemetry |
| registry.otel.traces.endpoint | string | `"http://terrakube-api.minikube.net:9411/api/v2/spans"` | endpoint its just example, change it to your jaeger endpoint |
| registry.podAnnotations | object | `{}` | Extra annotations for the pods |
| registry.podDisruptionBudget.enabled | bool | `false` | Create a PodDisruptionBudget |
| registry.podDisruptionBudget.minAvailable | int | `1` | Minimum available pods, integer or percentage. Mutually exclusive with maxUnavailable |
| registry.podLabels | object | `{}` | Extra labels for the pods |
| registry.readinessProbe | object | `{"periodSeconds":10}` | Readiness probe settings |
| registry.replicaCount | string | `"1"` | Number of Registry pod replicas |
| registry.resources | object | `{}` | Container resource requests and limits |
| registry.revisionHistoryLimit | int | `3` | Number of old ReplicaSets to keep |
| registry.secrets | list | `["terrakube-registry-secrets"]` | Secrets loaded into the container environment |
| registry.securityContext | object | `{}` | Pod security context for Registry |
| registry.serviceAccountName | string | `""` | Kubernetes Service Account name |
| registry.serviceAnnotations | object | `{}` | Annotations for the Service |
| registry.serviceType | string | `"ClusterIP"` | Kubernetes service type (ClusterIP/NodePort/LoadBalancer) |
| registry.startupProbe | object | `{"failureThreshold":30,"periodSeconds":5}` | Startup probe settings |
| registry.version | string | `""` | Terrakube Registry version |
| security | object | `{"adminGroup":"TERRAKUBE_ADMIN","dexClientId":"example-app","dexClientScope":"email openid profile offline_access groups","dexIssuerUri":"http://terrakube-api.minikube.net/dex","existingSecret":false,"internalSecret":"AxxPdgpCi72f8WhMXCTGhtfMRp6AuBfj","ldapConfigSecretName":"terrakube-openldap-secrets","patSecret":"bAUaAojZP3XhkuE2rWBtR3gRAHPzQKkx","useOpenLDAP":true}` | Azure Active Directory Security |
| security.adminGroup | string | `"TERRAKUBE_ADMIN"` | Admin group inside Terrakube |
| security.dexClientId | string | `"example-app"` | Based on Dex config file |
| security.dexClientScope | string | `"email openid profile offline_access groups"` | Use "email openid profile offline_access groups" |
| security.internalSecret | string | `"AxxPdgpCi72f8WhMXCTGhtfMRp6AuBfj"` | 32 Character secret to sing internal |
| security.patSecret | string | `"bAUaAojZP3XhkuE2rWBtR3gRAHPzQKkx"` | 32 Character secret to sign personal access token |
| storage | object | `{"aws":{"accessKey":"","bucketName":"","region":"","secretKey":""},"azure":{"storageAccountAccessKey":"","storageAccountName":"","storageAccountResourceGroup":""},"default":{"endpoint":"","region":"us-east-1"},"defaultStorage":true,"gcp":{"bucketName":"","credentials":"","projectId":""},"httpBackend":true,"minio":{"accessKey":"","bucketName":"","endpoint":"","secretKey":""}}` | Terraform Storage |
| storage.aws.accessKey | string | `""` | Aws access key |
| storage.aws.bucketName | string | `""` | Aws bucket name |
| storage.aws.region | string | `""` | Aws region name (Example: us-east-1) |
| storage.aws.secretKey | string | `""` | Aws secret key |
| storage.azure.storageAccountAccessKey | string | `""` | Azure storage access key |
| storage.azure.storageAccountName | string | `""` | Azure storage account name |
| storage.azure.storageAccountResourceGroup | string | `""` | Azure storage resource group |
| storage.defaultStorage | bool | `true` | Enable default storage using minio helm chart |
| storage.gcp.bucketName | string | `""` | GCP Bucket name for the storage |
| storage.gcp.credentials | string | `""` | GCP JSON Credentials for the storage |
| storage.gcp.projectId | string | `""` | GCP Project Id for the storage |
| storage.httpBackend | bool | `true` | Enable Terraform HTTP state backend (requires Terrakube 2.34.0+) When true, executor wraps state storage with HttpTerraformStateImpl and delegates state sync to Terrakube API |
| ui | object | `{"containerSecurityContext":{},"enabled":true,"existingSecret":false,"image":"azbuilder/terrakube-ui","imagePullSecrets":[],"initContainers":[],"podAnnotations":{},"podDisruptionBudget":{"enabled":false,"minAvailable":1},"podLabels":{},"replicaCount":"1","resources":{},"revisionHistoryLimit":3,"securityContext":{},"serviceAccountName":"","serviceAnnotations":{},"serviceType":"ClusterIP","version":""}` | UI Properties |
| ui.containerSecurityContext | object | `{}` | Container security context for UI |
| ui.enabled | bool | `true` | Enable UI component deployment |
| ui.existingSecret | bool | `false` | Use an existing Secret instead of the one the chart creates |
| ui.image | string | `"azbuilder/terrakube-ui"` | UI image repository |
| ui.imagePullSecrets | list | `[]` | Specific Secret used to pull images from private repository |
| ui.initContainers | list | `[]` | Init containers for UI deployment |
| ui.podAnnotations | object | `{}` | Extra annotations for the pods |
| ui.podDisruptionBudget.enabled | bool | `false` | Create a PodDisruptionBudget |
| ui.podDisruptionBudget.minAvailable | int | `1` | Minimum available pods, integer or percentage. Mutually exclusive with maxUnavailable |
| ui.podLabels | object | `{}` | Extra labels for the pods |
| ui.replicaCount | string | `"1"` | Number of UI pod replicas |
| ui.resources | object | `{}` | Container resource requests and limits |
| ui.revisionHistoryLimit | int | `3` | Number of old ReplicaSets to keep |
| ui.securityContext | object | `{}` | Pod security context for UI |
| ui.serviceAccountName | string | `""` | Kubernetes Service Account name |
| ui.serviceAnnotations | object | `{}` | Annotations for the Service |
| ui.serviceType | string | `"ClusterIP"` | Kubernetes service type (ClusterIP/NodePort/LoadBalancer) |
| ui.version | string | `""` | Terrakube UI version |
| valkey | object | `{"auth":{"aclUsers":{"default":{"password":"7p9iWVeRV4S944","permissions":"~* &* +@all"}},"enabled":true},"image":{"repository":"valkey/valkey","tag":""}}` | Default Valkey Configuration Uses the official Valkey Helm chart (https://github.com/valkey-io/valkey-helm). Authentication is ACL-based: the "default" user password is what Terrakube uses to connect (see api.properties.redisPort / TerrakubeRedisPassword). |
