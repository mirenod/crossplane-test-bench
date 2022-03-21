# Configure aws provider

Configure Secret
```sh
AWS_PROFILE=default && echo -e "[default]\naws_access_key_id = $(aws configure get aws_access_key_id --profile $AWS_PROFILE)\naws_secret_access_key = $(aws configure get aws_secret_access_key --profile $AWS_PROFILE)" > ./creds.conf

kubectl create secret generic aws-creds -n crossplane-system --from-file=creds=./creds.conf
```

Configure Provider
```sh
k apply -f provider-aws.yaml  
# Wait until the provider pod is ready
k get pods -A

# Configure provider
k apply -f provider-aws-config.yaml
```
