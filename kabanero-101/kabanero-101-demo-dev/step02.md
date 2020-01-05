Configure Kabanero and Test

## Target

Add a new Kabanero Collection to the Appsody repository.
[Kabanero Collection](https://github.com/kabanero-io/collections) represents the combination of an Appsody Stack + Tekton Pipeline.

Consider to use in your local Appsody repository the same Kabanero Collection version as defined in the Kabanero CustomResource in the remote cluster.

## Task

Add a Kabanero Collection Hub containing Appsody stacks

`export K_VERSION=0.4.0`{{execute}}

`appsody repo add kabanero https://github.com/kabanero-io/collections/releases/download/${K_VERSION}/kabanero-index.yaml`{{execute}}

Verify the installation and see the stacks from the *Kabanero* hub

`appsody list kabanero`{{execute}}

If you want to select the Kabanero repository as the default Appsody repository

`appsody repo set-default kabanero`{{execute}}

Verify that the Kabanero repository has now a * prefix

`appsody repo list`{{execute}}

## Verification

Verify that the same version of the Kabanero Collection is used. Login in OpenShift Cluster and check the Kabanero CR

`oc get kabanero -n kabanero -o yaml`{{execute}}

analyse the value of the field `spec.collections.repositories.url` and if your target namespace is observed/listed in `spec.targetNamespaces`.


## References

* Kabanero Collections: https://github.com/kabanero-io/collections
