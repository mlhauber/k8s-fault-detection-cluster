# Init Cluster

Pre: Cluster ready and kubectl configured

1. Install AlgoCD
```shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/mlhauber/k8s-fault-detection-cluster/main/init.yaml
```
2. Access AlgoCD GUI (optional)
User: admin
PW: see output of first command

```shell
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
3. Create Apps (Already injected for linkerd)
```shell
kubectl apply -f createArgocdApps.yaml
```


---
# Update external Dependencies
## Linkerd
### Win:
`linkerd install | Out-File -FilePath test.yaml`
### Linux:
`linkerd install | cat > test.yaml`
## Grafana loki
### Win
`helm template grafana/loki-stack | Out-File -FilePath test.yaml`
### Linux
`helm template grafana/loki-stack | cat > test.yaml`


## Apply Changes
```shell
kubectl apply -f createArgocdApps.yaml
```
