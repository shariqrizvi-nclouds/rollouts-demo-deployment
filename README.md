# Canary deployment using Argo Rollouts

Repository demonstrates implementation of Canary deployment using Argo Rollouts

Steps:

1. Create the Argo CD application using deployment repo https://github.com/shariqrizvi-nclouds/rollouts-demo-deployment: 

```
argocd app create rollouts-demo --repo https://github.com/shariqrizvi-nclouds/rollouts-demo-deployment.git --path . --dest-server https://kubernetes.default.svc --dest-namespace rollouts-demo --sync-policy automated
```

2. Make sure application is running by visiting http://rollouts-demo.apps.argoproj.io/.

3. Change image tag in `kustomization.yaml`. Available tags are `red`, `green`, `blue`, `yellow` and synchronize the change using Argo CD.

4. Verify that application is serving canary traffic and promote canary deployment.


Commands:

brew tap argoproj/tap

brew install argoproj/tap/argocd

kubectl create namespace rollouts-demo

kubectl create namespace argocd 

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

curl -LO https://github.com/argoproj/argo-rollouts/releases/download/v0.6.0/kubectl-argo-rollouts-darwin-amd64

chmod +x ./kubectl-argo-rollouts-darwin-amd64

sudo mv ./kubectl-argo-rollouts-darwin-amd64 /usr/local/bin/kubectl-argo-rollouts

helm install ngnix-ingress stable/nginx-ingress

kubectl apply -n argo-rollouts -f https://raw.githubusercontent.com/argoproj/argo-rollouts/stable/manifests/install.yaml

kubectl-argo-rollouts list rollouts --namespace rollouts-demo

### Login 

Port Forwarding: kubectl port-forward svc/argocd-server -n argocd 8080:443

Login: argocd login localhost:8080

usename: admin

Password: kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2

In chrome: https://localhost:8080/
