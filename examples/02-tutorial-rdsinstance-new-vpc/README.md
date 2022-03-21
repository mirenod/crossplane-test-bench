## Configure Provider

Configure aws provider, read config/crossplane/providers/README.md

## Install Crossplane configuration
```sh
k apply -f 01-xrd.yaml
# compositeresourcedefinition.apiextensions.crossplane.io/xpostgresqlinstances.database.example.org created
k apply -f 02-composition.yaml
# composition.apiextensions.crossplane.io/vpcpostgresqlinstances.aws.database.example.org created
```

## Deploy with Claim
```sh
k apply -f 03-claim.yaml

k get  postgresqlinstance.database.example.org/my-db
# NAME    READY   CONNECTION-SECRET   AGE
# my-db   False   db-conn             3m30s
```
```sh
k get crossplane
Warning: Please use v1beta1 version of SNS group.
NAME                                                                                                ESTABLISHED   OFFERED   AGE
compositeresourcedefinition.apiextensions.crossplane.io/xpostgresqlinstances.database.example.org   True          True      9h

NAME                                                                                      AGE
composition.apiextensions.crossplane.io/xpostgresqlinstances.aws.database.example.org     9h
composition.apiextensions.crossplane.io/vpcpostgresqlinstances.aws.database.example.org   5m52s

NAME                                      INSTALLED   HEALTHY   PACKAGE                           AGE
provider.pkg.crossplane.io/provider-aws   True        True      crossplane/provider-aws:v0.25.0   9h

NAME                                                           HEALTHY   REVISION   IMAGE                             STATE    DEP-FOUND   DEP-INSTALLED   AGE
providerrevision.pkg.crossplane.io/provider-aws-bec8ce984e49   True      1          crossplane/provider-aws:v0.25.0   Active                               9h

NAME                                             READY   SYNCED   ID                         VPC                     CIDR               AGE
subnet.ec2.aws.crossplane.io/my-db-vlb42-tqtf8   True    True     subnet-004077b73ced9ea7e   vpc-0f69e9113610e574e   192.168.64.0/18    4m27s
subnet.ec2.aws.crossplane.io/my-db-vlb42-tnqfw   True    True     subnet-05462ac0ad563708a   vpc-0f69e9113610e574e   192.168.192.0/18   4m27s
subnet.ec2.aws.crossplane.io/my-db-vlb42-rdhnd   True    True     subnet-00780b73ce8883da8   vpc-0f69e9113610e574e   192.168.128.0/18   4m27s

NAME                                                 READY   SYNCED   ID                      VPC                     AGE
routetable.ec2.aws.crossplane.io/my-db-vlb42-2f7wb   True    True     rtb-066e9904f47b2f49b   vpc-0f69e9113610e574e   4m27s

NAME                                                      READY   SYNCED   ID                      VPC                     AGE
internetgateway.ec2.aws.crossplane.io/my-db-vlb42-gt488   True    True     igw-013fa5bc097e1d582   vpc-0f69e9113610e574e   4m27s

NAME                                          READY   SYNCED   ID                      CIDR             IPV6CIDR   AGE
vpc.ec2.aws.crossplane.io/my-db-vlb42-j8vmd   True    True     vpc-0f69e9113610e574e   192.168.0.0/16              4m27s

NAME                                                    READY   SYNCED   ID                     VPC                     AGE
securitygroup.ec2.aws.crossplane.io/my-db-vlb42-r9nq9   True    True     sg-067741b50d5e03cce   vpc-0f69e9113610e574e   4m27s

NAME                                       AGE
providerconfig.aws.crossplane.io/default   9h

NAME                                                                         AGE     CONFIG-NAME   RESOURCE-KIND     RESOURCE-NAME
providerconfigusage.aws.crossplane.io/d92dc571-7566-43b2-823e-75c67b75695f   4m27s   default       VPC               my-db-vlb42-j8vmd
providerconfigusage.aws.crossplane.io/49e63925-3a00-4642-b306-1a9273956fa3   4m26s   default       Subnet            my-db-vlb42-tqtf8
providerconfigusage.aws.crossplane.io/19491386-8210-4412-83f1-fc11713ac0f7   4m26s   default       InternetGateway   my-db-vlb42-gt488
providerconfigusage.aws.crossplane.io/1dbf772b-ad65-460b-871d-8dfb0464753b   4m25s   default       Subnet            my-db-vlb42-rdhnd
providerconfigusage.aws.crossplane.io/68125d7b-617a-4ac0-9ac7-28b759297fb9   4m24s   default       Subnet            my-db-vlb42-tnqfw
providerconfigusage.aws.crossplane.io/b0a21a11-fd30-49c0-ac61-06ba8b3f848f   4m21s   default       RouteTable        my-db-vlb42-2f7wb
providerconfigusage.aws.crossplane.io/d1b840df-a9c9-4130-91c4-cf4eb5d1c58f   4m18s   default       SecurityGroup     my-db-vlb42-r9nq9
providerconfigusage.aws.crossplane.io/7bd366ba-fe3b-483b-865b-d6d26d955a3f   4m15s   default       RDSInstance       my-db-vlb42-mzcdt
providerconfigusage.aws.crossplane.io/b5730f11-ee27-495d-8b90-2218417a2a28   4m9s    default       DBSubnetGroup     my-db-vlb42-qjk4m

NAME                                                         READY   SYNCED   AGE
dbsubnetgroup.database.aws.crossplane.io/my-db-vlb42-qjk4m   True    True     4m27s

NAME                                                       READY   SYNCED   STATE      ENGINE     VERSION   AGE
rdsinstance.database.aws.crossplane.io/my-db-vlb42-mzcdt   False   True     creating   postgres   12.8      4m27s
```

# Delete Claim
```sh
k delete  postgresqlinstance.database.example.org/my-db
```
