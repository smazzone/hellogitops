


kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


argocd login --core

argocd cluster add rancher-desktop

kubectl config set-context --current --namespace=argocd

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

argocd admin initial-password -n argocd

argocd app create hello-world --repo https://github.com/smazzone/hellogitops --path manifests --dest-server https://kubernetes.default.svc --dest-namespace default

argocd app set hello-world --sync-policy automated

