## Configure Provider

Configure aws provider, read config/crossplane/providers/README.md

## Install Crossplane configuration
```sh
k apply -f 01-xrd.yaml
# compositeresourcedefinition.apiextensions.crossplane.io/xpostgresqlinstances.database.example.org created
k apply -f 02-composition.yaml
# composition.apiextensions.crossplane.io/xpostgresqlinstances.aws.database.example.org created
```

## Deploy with Claim
```sh
k apply -f 03-claim.yaml

k get  rdsinstance.database.aws.crossplane.io/my-db-7h25m-j6475
# NAME                READY   SYNCED   STATE        ENGINE     VERSION   AGE
# my-db-7h25m-j6475   True    True     backing-up   postgres   12.8      5m13s
```
## Delete Claim
```sh
k delete PostgreSQLInstance my-db
```
