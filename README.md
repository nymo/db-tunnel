# db-tunnel

Helm deployment for a [socat](https://hub.docker.com/r/alpine/socat) TCP database tunneler.

The chart creates a `Deployment` and an in-cluster `ClusterIP` `Service`. Connections to the Service are forwarded to the database host supplied through `dbhost`.

## Prerequisites

- [Helm 3](https://helm.sh/docs/intro/install/)
- Access to a Kubernetes cluster

## Install

Set `dbhost` when installing the chart:

```sh
helm upgrade --install db-tunnel ./charts/socat-tunneler \
  --set-string dbhost=db.example.internal
```

The tunnel is available inside the cluster at `db-tunnel-socat-tunneler:5432` by default. Configure applications to use that Service name and port instead of connecting to the database directly.

For a database using a different port, set `dbport`:

```sh
helm upgrade --install db-tunnel ./charts/socat-tunneler \
  --set-string dbhost=db.example.internal \
  --set dbport=3306
```

Use `listenPort` to change the port exposed by the tunnel inside the cluster. The Service port can be changed independently with `service.port`.

## Image pinning

The chart uses `alpine/socat:1.8.1.3`, the newest version reported by Docker Hub when this chart was created, pinned to its Docker Hub manifest digest:

```text
alpine/socat:1.8.1.3@sha256:3d9e7966201dd3a065df591020a09fd3c70845de7e7086e3531ea69db774406b
```

Update both `image.tag` and `image.digest` in `charts/socat-tunneler/values.yaml` when upgrading the image.

## Uninstall

```sh
helm uninstall db-tunnel
```
