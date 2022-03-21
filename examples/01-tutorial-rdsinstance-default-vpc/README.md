## Configure Provider

Configure Secret
```sh
AWS_PROFILE=default && echo -e "[default]\naws_access_key_id = $(aws configure get aws_access_key_id --profile $AWS_PROFILE)\naws_secret_access_key = $(aws configure get aws_secret_access_key --profile $AWS_PROFILE)" > ../../config/crossplane/providers/creds.conf

kubectl create secret generic aws-creds -n crossplane-system --from-file=creds=../../config/crossplane/providers/creds.conf
```

Configure Provider
```sh
k apply -f ../../config/crossplane/providers/provider-aws.yaml  
# Wait until the provider pod is ready
k get pods -A

# Configure provider
k apply -f ../../config/crossplane/providers/provider-aws-config.yaml
```

## Install Crossplane configuration
```sh
k apply -f 01-xrd.yaml
# compositeresourcedefinition.apiextensions.crossplane.io/xpostgresqlinstances.database.example.org created
k apply -f 02-composition.yaml
# composition.apiextensions.crossplane.io/xpostgresqlinstances.aws.database.example.org created
```
