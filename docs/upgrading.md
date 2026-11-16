# Upgrading

Always run `helm repo update` and review the values you override against the [values reference](../charts/terrakube/README.md#values).

## 4.7.18

- The init container restart policy is configurable with `executor.initRestartPolicy`. `Always` uses a native sidecar on Kubernetes 1.29+.

## 4.7.16

- Executor secrets are loaded through an in-memory `emptyDir` by default (`executor.emptyDirSecret: true`). This needs executor image 2.34.0 or later. Set the executor and API versions together.

## 4.7.0

- Bitnami Redis is replaced by the Valkey chart. The `redis` values block is now `valkey`.
- The default Redis password moved from `redis.auth.password` to `valkey.auth.aclUsers.default.password`.
- The Redis service name changed from `<release>-redis-master` to `<release>-valkey`. Update anything that connects to the bundled Redis directly.
- External Redis (`api.defaultRedis: false`) is unchanged.

## 2.0.0

- Authentication uses Dex. It needs Terrakube 2.6.0 or later. Values from earlier chart versions are not compatible.
