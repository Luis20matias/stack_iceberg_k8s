# GitOps using ArgoCD

### install
```sh
brew install helm
brew install argocd
```

### repos
```sh
helm repo add argo https://argoproj.github.io/argo-helm
```

## Start Minikube

```sh
minikube start -p ice-inno --nodes=3 --cpus=4 --memory=4000
```

Run the following commands to make the pvc fine locally (remmeber to update it in your values.yaml):

```bash
minikube addons enable csi-hostpath-driver -p ice-inno
minikube addons enable volumesnapshots -p ice-inno
minikube addons enable ingress -p ice-inno
minikube addons enable ingress-dns -p ice-inno
minikube addons enable metrics-server -p ice-inno
```

### build
```sh
terraform init
terraform apply
```

### configure [manual]
```sh
kubectl patch svc argocd-server -n gitops -p '{"spec": {"type": "LoadBalancer"}}'
kubectl -n gitops get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
kubectl port-forward svc/argocd-server -n gitops 8080:443

# Update the password and enable use argo in your terminal (password is the second command from the list on the top)
argocd login "localhost:8080" --username admin --password "afElsutmZYEvo75R" --insecure

# Create argo cluster
argocd cluster add "ice-inno"

# Connect with Github
kubectl apply -f git-repo-con.yaml -n gitops


Allow Loadbalancer
```bash
minikube tunnel -p ice-inno
```

# Optional Commands to create the cluster
Generate the token to be added in the secret
kubectx
kubectl config view --context ice-inno --minify --flatten -o json | base64 -w 0
echo -n "ice-inno" | base64 # update the name
kubectl apply -f secret.yaml # don't forget to update the secret.yaml with the base64 output, on fields name and config respectively
```
