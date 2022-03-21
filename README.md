## Configure Repo
```sh
k3d cluster create --config config/k3d.yaml
```

Install Crossplane
```sh
# https://crossplane.io/docs/v1.6/getting-started/install-configure.html
kubectl create namespace crossplane-system

helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update

helm install crossplane --namespace crossplane-system crossplane-stable/crossplane
```


## Configure Providers
Create secret
```sh
kubectl create secret generic aws-creds -n crossplane-system --from-file=creds=./config/crossplane/provider/creds.conf
```


```sh
k apply -f config/crossplane/provider/provider-aws.yaml
k apply -f config/crossplane/provider/provider-jet-aws.yaml
# Config is going to fail until the provider have not configure the kind
```
