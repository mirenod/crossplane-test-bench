## Install configuration
```sh
kubectl crossplane install configuration registry.upbound.io/xp/getting-started-with-aws:v1.6.4 

watch kubectl get pkg 
# NAME                                                          INSTALLED   HEALTHY   PACKAGE                                                  AGE
# configuration.pkg.crossplane.io/xp-getting-started-with-aws   True        True      registry.upbound.io/xp/getting-started-with-aws:v1.6.4   84s

# NAME                                                 INSTALLED   HEALTHY   PACKAGE                           AGE
# provider.pkg.crossplane.io/crossplane-provider-aws   True        True      crossplane/provider-aws:v0.25.0   81s
```

# Configutre provider
```sh
cd examples/00-tutrorial-using-configuration-package/
AWS_PROFILE=default && echo -e "[default]\naws_access_key_id = $(aws configure get aws_access_key_id --profile $AWS_PROFILE)\naws_secret_access_key = $(aws configure get aws_secret_access_key --profile $AWS_PROFILE)" > creds.conf

kubectl create secret generic aws-creds -n crossplane-system --from-file=creds=./creds.conf
```

```sh
k apply -f 00-provider-config.yaml
```

## Deploy Claim
```sh
k apply -f 01-claim.yaml
```

### Ways to get the claim
```sh
kubectl get postgresqlinstance my-db
NAME    READY   CONNECTION-SECRET   AGE
my-db   False   db-conn             11s

kubectl get crossplane -l crossplane.io/claim-name=my-db
Warning: Please use v1beta1 version of SNS group.
NAME                                                       READY   SYNCED   STATE      ENGINE     VERSION   AGE
rdsinstance.database.aws.crossplane.io/my-db-d9jcs-dkdp6   False   True     creating   postgres   12.8      22s
```

```
k get crossplane
Warning: Please use v1beta1 version of SNS group.
NAME                                                                                                ESTABLISHED   OFFERED   AGE
compositeresourcedefinition.apiextensions.crossplane.io/xpostgresqlinstances.database.example.org   True          True      11m

NAME                                                                                    AGE
composition.apiextensions.crossplane.io/xpostgresqlinstances.aws.database.example.org   11m

NAME                                                 INSTALLED   HEALTHY   PACKAGE                           AGE
provider.pkg.crossplane.io/crossplane-provider-aws   True        True      crossplane/provider-aws:v0.25.0   11m

NAME                                                                               HEALTHY   REVISION   IMAGE                                                    STATE    DEP-FOUND   DEP-INSTALLED   AGE
configurationrevision.pkg.crossplane.io/xp-getting-started-with-aws-6acb3f709c6c   True      1          registry.upbound.io/xp/getting-started-with-aws:v1.6.4   Active   1           1               11m

NAME                                                                      HEALTHY   REVISION   IMAGE                             STATE    DEP-FOUND   DEP-INSTALLED   AGE
providerrevision.pkg.crossplane.io/crossplane-provider-aws-bec8ce984e49   True      1          crossplane/provider-aws:v0.25.0   Active                               11m

NAME                                                          INSTALLED   HEALTHY   PACKAGE                                                  AGE
configuration.pkg.crossplane.io/xp-getting-started-with-aws   True        True      registry.upbound.io/xp/getting-started-with-aws:v1.6.4   11m

NAME                                       AGE
providerconfig.aws.crossplane.io/default   5m13s

NAME                                                                         AGE     CONFIG-NAME   RESOURCE-KIND   RESOURCE-NAME
providerconfigusage.aws.crossplane.io/44f0b5c9-5d75-40f8-88e4-e3fc569a3088   3m49s   default       RDSInstance     my-db-d9jcs-dkdp6

NAME                                                       READY   SYNCED   STATE      ENGINE     VERSION   AGE
rdsinstance.database.aws.crossplane.io/my-db-d9jcs-dkdp6   False   True     creating   postgres   12.8      3m49s
```

## Delte Claim
```sh
k delete -f 01-claim.yaml   

k get rdsinstance.database.aws.crossplane.io/my-db-d9jcs-dkdp6
# NAME                READY   SYNCED   STATE      ENGINE     VERSION   AGE
# my-db-d9jcs-dkdp6   False   True     deleting   postgres   12.8      9m7s
```


## Cleaning configuration
```sh
k get configuration.pkg
# NAME                          INSTALLED   HEALTHY   PACKAGE                                                  AGE
# xp-getting-started-with-aws   True        True      registry.upbound.io/xp/getting-started-with-aws:v1.6.4   18m

k delete configuration.pkg xp-getting-started-with-aws

# Delete the configuration but the provider that was install by the config is still there.
k delete -f 00-provider-config.yaml
# k get pkg
# NAME                                                 INSTALLED   HEALTHY   PACKAGE                           AGE
# provider.pkg.crossplane.io/crossplane-provider-aws   True        True      crossplane/provider-aws:v0.25.0   23m
k delete provider.pkg.crossplane.io/crossplane-provider-aws
```
