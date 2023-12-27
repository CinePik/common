# common

Common files and configs for all CinePik microservices.

## Kubernetes deployment

### Apply changes

We can create the ingress controller.

```bash
kubectl apply -f k8s/cinepik-catalog.yml
```

### Other useful commands

```bash
kubectl get pods
kubectl delete deployment cinepik-keycloak-deployment
kubectl delete configmap <configmap name>
kubectl rollout restart deployment/cinepik-keycloak-deployment
kubectl logs <pod-id>
kubectl describe secret <secret-name>
kubectl get secret <secret-name>
```
