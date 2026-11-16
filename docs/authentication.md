# Authentication

## Requirements

Please make sure to have a kubernetes setup with some ingress setup completed for example Nginx Ingress, but any other kubernetes ingress should work. Gateway API is also supported for example NGINX Gateway Fabric, but any other Gateway API implementation should work as well.

## Dex connectors

To handle authentication we use [DEX](https://dexidp.io/) to support different providers using [connectors](https://dexidp.io/docs/connectors/), you can use any connectors as long it supports the ***groups scope***.
For example you can use the following connectors:
- LDAP
- GitHub
- SAML 2.0
- Gitlab
- OpenID Connect
- Google
- Microsoft
- OpenShift
- Etc.

> Dex authentication is only supported from Terrakube 2.6.0 and chart version 2.0.0. The chart values are not backward compatible with lower versions.

Once we have decided which connectors we would like to use we can create Dex configuration.
The following in an example of Dex configuration using Azure Active Directory, Google Cloud Identity, Github and Gitlab to handle authentication and groups:

```yaml
    config:
      issuer: https://terrakube-api.domain.com/dex
      storage:
        type: memory
      oauth2:
        responseTypes: ["code", "token", "id_token"]
      web:
        allowedOrigins: ['*']

      staticClients:
      - id: microsoft
        redirectURIs:
        - 'https://terrakube-ui.domain.com'
        - 'http://localhost:10001/login' # required for terraform login
        - 'http://localhost:10000/login' # required for terraform login
        - '/device/callback'
        name: 'microsoft'
        public: true
      - id: github
        redirectURIs:
        - 'https://terrakube-ui.domain.com'
        - 'http://localhost:10001/login' # required for terraform login
        - 'http://localhost:10000/login' # required for terraform login
        - '/device/callback'
        name: 'github-web'
        public: true
      - id: gitlab
        redirectURIs:
        - 'https://terrakube-ui.domain.com'
        - 'http://localhost:10001/login' # required for terraform login
        - 'http://localhost:10000/login' # required for terraform login
        - '/device/callback'
        name: 'gitlab'
        public: true
      - id: google
        redirectURIs:
        - 'https://terrakube-ui.domain.com'
        - 'http://localhost:10001/login' # required for terraform login
        - 'http://localhost:10000/login' # required for terraform login
        - '/device/callback'
        name: 'google'
        public: true

      connectors:
      - type: microsoft
        id: microsoft
        name: microsoft
        config:
          clientID: "<<CLIENT ID FROM FROM AZURE ACTIVE DIRECTORY>>"
          clientSecret: "<<CLIENT SECRET FROM AZURE ACTIVE DIRECTORY>>"
          redirectURI: "https://api.terrakube.docker.internal/dex/callback"
          tenant: "<<TENANT ID FROM AZURE ACTIVE DIRECTORY>>"
      - type: github
        id: github
        name: gitHub
        config:
          clientID: "<<CLIENT ID FROM GITHUB>>"
          clientSecret: "<<CLIENTE SECRET FROM GITHUB>>"
          redirectURI: "https://api.terrakube.docker.internal/dex/callback"
      - type: gitlab
        id: gitlab
        name: gitLab
        config:
          clientID: "<<CLIENT ID FROM GITLAB>>"
          clientSecret: "<<CLIENT SECRET FROM GITLAB>>"
          redirectURI: "https://api.terrakube.docker.internal/dex/callback"
      - type: google
        id: google
        name: google
        config:
          clientID: "<<CLIENT ID FROM GOOGLE CLOUD>>"
          clientSecret: "<<CLIENT SECRET FROM GOOGLE CLOUD>>"
          redirectURI: "https://api.terrakube.docker.com/dex/callback"
          serviceAccountFilePath: "/etc/secrets/gcp-credentials.json"
          adminEmail: "superAdmin@demo.com"
```

To learn more about how to build the Dex configuration file please review the following [documentation](https://dexidp.io/docs/)

- [Azure Active Directory](https://dexidp.io/docs/connectors/microsoft/)
- [Google Cloud Identity](https://dexidp.io/docs/connectors/google/)
- [Github](https://dexidp.io/docs/connectors/github/)
- [Gitlab](https://dexidp.io/docs/connectors/gitlab/)

## OIDC discovery and JWKS endpoints

Terrakube exposes the following public OIDC endpoints through the API service for authentication and JWT token validation:

- `/.well-known/openid-configuration` - OpenID Connect discovery endpoint
- `/.well-known/jwks` - JSON Web Key Set (JWKS) endpoint for JWT signature verification

These endpoints are automatically configured as **public** (without security policies) across all supported ingress types:
- **Generic/NGINX Ingress**: Public paths with exact matching
- **GKE Ingress**: Dedicated BackendConfig without Cloud Armor security policies
- **Gateway API (HttpRoute)**: Public routes with exact path matching

This allows external services and clients to:
- Discover OIDC configuration automatically
- Validate JWT tokens issued by Terrakube
- Integrate with third-party authentication systems

The endpoints are exposed on your configured API domain:
```
https://terrakube-api.example.com/.well-known/openid-configuration
https://terrakube-api.example.com/.well-known/jwks
```

## Terrakube admin group and internal security

In order to use Terrakube you will have to define the one administrator group, for example:
- TERRAKUBE_ADMIN

Members of these groups are the only users inside terrakube that can create ***organizations*** and ***handle team access***

Example:

```yaml
security:
  adminGroup: "TERRAKUBE_ADMIN"
  patSecret: "<<CHANGE_THIS>>"  # Sample Key 32 characters z6QHX!y@Nep2QDT!53vgH43^PjRXyC3X
  internalSecret: "<<CHANGE_THIS>>" # Sample Key 32 characters Kb^8cMerPNZV6hS!9!kcD*KuUPUBa^B3
```

> The patSecret and internalSecret should be a 32 character base64 compatible string.

> For Google Cloud Identity groups name will be like "groupName@yourdomain.com", For Github Authentication will be like "organizationName:teamName"
