# thesis-ladeplattform

Pre: Cluster ready and kubectl configured

1. Install AlgoCD
```shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
2. Access AlgoCD GUI
User: admin
PW: see output of first command

```shell
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
3. Create Apps (Already injected for linkerd)
```yaml
Name: servicemesh
project: default
source:
repoURL: 'https://github.com/mlhauber/k8s-fault-detection-cluster.git'
path: linkerd
targetRevision: main
destination:
server: 'https://kubernetes.default.svc'
namespace: servicemesh
syncPolicy:
automated:
  prune: true
  selfHeal: true
syncOptions:
  - CreateNamespace=true
---
  
  - Name: logging
    Repo: https://github.com/mlhauber/k8s-fault-detection-cluster.git
    Path: loki
    Branch: Master
    NameSpace: logging
```
