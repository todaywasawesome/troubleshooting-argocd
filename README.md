# troubleshooting-argocd


## Setup
Create cluster with kind `kind create cluster --kubeconfig ~/.kube/troubleshoot --name troubleshoot`

Export kubeconfig `export KUBECONFIG=~/.kube/troubleshoot`
Export server url `export KUBEURL=$(kubectl config view --minify --output jsonpath="{.clusters[*].cluster.server}" | sed 's|^https://||' | tr -d '/')`

Install argocd `kubectl create namespace argocd` `kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml`

Install argocd cli `brew install argocd`

Export Argo password `export ARGO_PWD=$(kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d)`

Port forward to argocd (runs in background and auto-restarts):
```bash
nohup bash -c 'while true; do kubectl port-forward svc/argocd-server -n argocd 8080:80; sleep 5; done' > /dev/null 2>&1 &
echo $! > .argocd-port-forward.pid
```

To stop the port-forwarding:
```bash
kill $(cat .argocd-port-forward.pid)
rm .argocd-port-forward.pid
```
Login to argocd `argocd login localhost:8080 --username admin --password $ARGO_PWD --insecure`

## Example Apps
Install the broken version of apps: 
`for f in example-apps/**/*-broken.yaml; do kubectl apply -f $f; done`


### 05 Cluster Issues

Create a cluster with kind `kind create cluster --kubeconfig ~/.kube/remotecluster --name remotecluster`

Copy the kubeconfig to the remote cluster `cp ~/.kube/remotecluster ~/.kube/remotecluster.broken` and edit the server url to `https://127.0.0.1`

Add the broken remote cluster to ArgoCD `argocd cluster add kind-remotecluster --kubeconfig ~/.kube/remotecluster.broken --namespace default` 