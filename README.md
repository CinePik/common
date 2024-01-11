# common

Common files and configs for all CinePik microservices.

Useful resources:

- [Managed nginx Ingress with the application routing add-on](https://learn.microsoft.com/en-us/azure/aks/app-routing?tabs=default%2Cdeploy-app-default)

## Docker
### Docker Compose
    
```bash
# Logging
docker-compose -f docker-compose-logging.yml up -d --build
docker-compose -f docker-compose-logging.yml down
```

## Kubernetes deployment

## Setup

### Apply changes

Create the ingress controller.

```bash
kubectl apply -f k8s/ingress.yml
```

### Other useful commands

```bash
kubectl get ingress
kubectl get pods -o wide
kubectl get service
kubectl describe ingress cinepik-ingress
kubectl delete ingress cinepik-ingress
```
