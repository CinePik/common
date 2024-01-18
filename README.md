# common

Common files and configs for all CinePik microservices.

Useful resources:

- Create ingress controller in AKS: [Managed nginx Ingress with the application routing add-on](https://learn.microsoft.com/en-us/azure/aks/app-routing?tabs=default%2Cdeploy-app-default)
- Deploy Prometheus in k8s: [How to Setup Prometheus Monitoring On Kubernetes Cluster](https://devopscube.com/setup-prometheus-monitoring-on-kubernetes/)
- Deploy Grafana in k8s: [How To Setup Grafana On Kubernetes](https://devopscube.com/setup-grafana-kubernetes/)
- Create and use a volume in AKS: [Create and use a volume with Azure Disks in Azure Kubernetes Service (AKS)](https://learn.microsoft.com/en-us/azure/aks/azure-csi-disk-storage-provision)

## Docker

### Docker Compose

```bash
# Logging
docker-compose -f docker-compose.logging.yml up -d --build
docker-compose -f docker-compose.logging.yml down
```

## Kubernetes deployment

### Monitoring

Create namespace

```bash
kubectl apply -f /monitoring/k8s/namespace.yml
```

#### Prometheus

Create cluster role for Prometheus

```bash
kubectl apply -f /monitoring/k8s/prometheus-role.yml
```

Create config map for Prometheus

```bash
kubectl apply -f /monitoring/k8s/prometheus-configmap.yml
```

Create persistent volume claim for Prometheus

```bash
kubectl apply -f /monitoring/k8s/prometheus-pvc.yml
```

Create deployment and service for Prometheus

```bash
kubectl apply -f /monitoring/k8s/prometheus-deployment.yml
```

#### Grafana

 Create config map for Grafana

```bash
kubectl apply -f /monitoring/k8s/grafana-configmap.yml
```

Create secret for Grafana

```bash
kubectl create secret generic grafana-secret --namespace monitoring \
  --from-literal=admin-user='admin' \
  --from-literal=admin-password='<REPLACE_ME>'
```

Create persistent volume claim for Grafana

```bash
kubectl apply -f /monitoring/k8s/grafana-pvc.yml
```

Create deployment and service for Grafana

```bash
kubectl apply -f /monitoring/k8s/grafana-deployment.yml
```

## Setup

### Routing

Create default ingress controller.

```bash
kubectl apply -f k8s/ingress.yml
```

Create monitoring ingress controller. Make sure to create the monitoring namespace first.

```bash
kubectl create namespace monitoring
```

Since each app expects traffic to be on `/` some app redirects cannot be handled correctly by ingress, i.e. all prometheus traffic needs to be on `/prometheus/something`.
The best solution would be to solve this in the ingress itself, but not all solutions fix everything. Currently using the rewrite-target annotation works for Prometheus, but not for Grafana, so the current fix for Grafana is to use an environment variable `GF_SERVER_ROOT_URL` to set the base path.

```bash
kubectl apply -f k8s/ingress-monitoring.yml
```

### Other useful commands

```bash
kubectl get ingress
kubectl get pods -o wide
kubectl get service
kubectl describe ingress cinepik-ingress
kubectl delete ingress cinepik-ingress
```
