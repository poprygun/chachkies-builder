# Add application to Argo CD

Follow [Getting Started Guide](https://argoproj.github.io/argo-cd/getting_started/) to setup Argo CD

Use  Port Forwarding to access [application console](https://localhost:8080/applications)

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

## Create application

```bash
argocd app create chachkies --repo https://github.com/poprygun/chachkies-builder.git --path argo/chachkies --dest-server https://kubernetes.default.svc --dest-namespace default --directory-recurse
```

## Verify application health

```bash
kubectl run busybox --image=busybox --rm -it --restart=Never -- wget -qO- chachkies-service:3000/graphiql
```
