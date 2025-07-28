
Follow ArgoCD [getting started guide](https://argo-cd.readthedocs.io/en/stable/getting_started/#getting-started)

to install ArgoCD on your K8s cluster:

```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

install the argo cd cli and configure it:

```
brew install argocd
argocd login --core
```

switch to the proper K8s context and register a cluster to deploy your apps:

```
kubectl config get-contexts -o name

kubectl config set-context --current --namespace=argocd

argocd cluster add rancher-desktop
```

change the argocd-server service type to LoadBalancer:

```
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

retrieve auto-generated password using the argocd CLI:

```
argocd admin initial-password -n argocd
```

switch to the proper K8s context and create an app in ArgoCD using the argo cli:

```
kubectl config set-context --current --namespace=argocd

argocd app create hello-world --repo https://github.com/smazzone/hellogitops --path manifests --dest-server https://kubernetes.default.svc --dest-namespace default
```


to configure automated sync run:

```
argocd app set hello-world --sync-policy automated
```

NB:The default argocd polling interval is 3 minutes (180 seconds)
