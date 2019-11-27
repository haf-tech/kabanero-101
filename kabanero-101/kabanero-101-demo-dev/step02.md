Configure and Test

## Target

Add a new Kabanero Collection to the Appsody repository.
[Kababer Collection](https://github.com/kabanero-io/collections) represents the combination of an Appsody Stack + Tekton Pipeline.

## Task

Add a Kabanero Collection Hub containing Appsody stacks

`appsody repo add kabanero https://github.com/kabanero-io/collections/releases/download/0.3.0/kabanero-index.yaml`{{execute}}

Verify the installation and see the stacks from the *Kabanero* hub

`appsody list kabanero`{{execute}}

If you want to select the Kabanero repository as the default Appsody repository

`appsody repo set-default kabanero`{{execute}}

Verify that the Kabanero repository has a * prefix

`appsody repo list`{{execute}}
