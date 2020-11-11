# Add application to Argo CD

```bash
argocd app create guestbook --repo https://github.com/poprygun/chachkies-builder.git --path chachkies --dest-server https://kubernetes.default.svc --dest-namespace default
```