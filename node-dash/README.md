# Device Manager Helm Chart

This Helm chart deploys the Device Manager application and its components in a Kubernetes cluster.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.0+
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
helm install my-release ./helm/device-manager
```

## Configuration

The following table lists the configurable parameters of the Device Manager chart and their default values.

### Global Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `global.registry` | Global Docker registry | `registry.heliumdevicemanager.com` |
| `global.imageTag` | Global Docker image tag | `0.0.8` |

### Frontend (UX) Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `ux.enabled` | Enable UX deployment | `true` |
| `ux.image.repository` | UX image repository | `device-manager/device-manager-ux` |
| `ux.service.type` | UX service type | `ClusterIP` |
| `ux.service.port` | UX service port | `3000` |
| `ux.env.VITE_API_MODE` | API mode | `live` |
| `ux.env.VITE_API_URL` | API URL | `http://localhost:8081` |

### API Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `api.enabled` | Enable API deployment | `true` |
| `api.image.repository` | API image repository | `device-manager/device-manager-api` |
| `api.service.type` | API service type | `ClusterIP` |
| `api.service.port` | API service port | `8000` |
| `api.env.SECRET_KEY` | API secret key | `your_secure_secret_key` |

### Database Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `postgresql.enabled` | Enable PostgreSQL deployment | `true` |
| `postgresql.auth.username` | PostgreSQL username | `postgres` |
| `postgresql.auth.password` | PostgreSQL password | `postgres` |
| `postgresql.auth.database` | PostgreSQL database name | `device_manager` |
| `postgresql.primary.persistence.size` | PostgreSQL PVC size | `8Gi` |

### Valkey Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `valkey.enabled` | Enable Valkey deployment | `true` |
| `valkey.image.repository` | Valkey image repository | `valkey/valkey` |
| `valkey.image.tag` | Valkey image tag | `8.1.1-alpine3.21` |
| `valkey.persistence.size` | Valkey PVC size | `8Gi` |

## Usage

After the chart is installed, you can access the application through the frontend service. By default, the application is accessible through a ClusterIP service. You may want to configure an Ingress or use port forwarding to access it from outside the cluster.

To use port forwarding:

```bash
kubectl port-forward svc/my-release-ux 8080:3000
```

Then access the application at `http://localhost:8080`

## Upgrading

To upgrade the release:

```bash
helm upgrade my-release ./helm/device-manager
```

## Uninstalling

To uninstall/delete the release:

```bash
helm uninstall my-release
```

This will delete all the Kubernetes resources associated with the chart and deletes the release.