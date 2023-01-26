# Create k8s cluster
1. create the cluster using Kind
```
kind create cluster --config 
```

2. Test the cluster 
```
kubectl get node
```
#

# Install argo-cd 
1. Add repository
```
helm repo add argo https://argoproj.github.io/argo-helm
```
2. update the helm repositories
```
helm repo update 
```
3. Install chart for argo-cd
```
helm install my-argo-cd argo/argo-cd --version 5.19.8
```
4. You can find the password by running:
```
kubectl -n default get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

eXPnsyBmx0zRxAwj
```
5. In order to access the server UI you have the following options:

```
kubectl port-forward service/my-argo-cd-argocd-server -n default 8080:443

```
    then open the browser on http://localhost:8080 and accept the certificate , user admin 
#

# Install crossplane
1. create crossplane namesapce

```
kubectl create namespace crossplane-system
```

2. Add and updaterepository
```
helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update
```
3. Install crossplane chart
```
helm install crossplane --namespace crossplane-system crossplane-stable/crossplane
```

## OR with Arogcd
```YAML
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: crossplane-system
spec:
  destination:
    name: ''
    namespace: crossplane-system
    server: 'https://kubernetes.default.svc'
  source:
    path: ''
    repoURL: 'https://charts.crossplane.io/master/'
    targetRevision: 1.11.0-rc.0.352.g4a6a8534
    chart: crossplane
  project: default
  syncPolicy:
    automated:
      prune: false
      selfHeal: false
    syncOptions:
      - CreateNamespace=true
```
4. Install crossplane sealed
```
helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets

helm repo update

helm install my-sealed-secrets bitnami-labs/sealed-secrets --version 2.7.3
```

