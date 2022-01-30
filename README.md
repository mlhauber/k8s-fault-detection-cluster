# thesis-ladeplattform

Pre: Cluster ready and kubectl configured

1. Install AlgoCD
´´´shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
´´´
2. Access AlgoCD GUI
´´´shell
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
kubectl port-forward svc/argocd-server -n argocd 8080:443
´´´
3. Create Apps
  - Name: servicemesh
    Repo: https://github.com/mlhauber/k8s-fault-detection-cluster
    Path: linkerd
    Branch: Master
    NameSpace: servicemesh
  - Name: logging
    Repo: https://github.com/mlhauber/k8s-fault-detection-cluster
    Path: linkerd
    Branch: Master
    NameSpace: logging
