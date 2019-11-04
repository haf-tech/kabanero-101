Install Kabanero Foundation.

Kabanero CLI manages the Kabanero collections.

## Task

Clone the git repository with the installation scripts (branch/version 0.2.0)

`mkdir -p /tmp/kabanero-foundation && cd /tmp/kabanero-foundation && git clone -b 0.2.0 --single-branch  https://github.com/kabanero-io/kabanero-foundation.git && cd kabanero-foundation/scripts`{{execute}}

Install Kabanero Foundation, with the following paramters
* Set the Subdomain
* Enable kAppNav

``ENABLE_KAPPNAV=yes openshift_master_default_subdomain=[[HOST_SUBDOMAIN]]-8443-[[KATACODA_HOST]].environments.katacoda.com ./install-kabanero-foundation.sh``{{execute}}

Install the Kabanero Custom Resource

`oc apply -n kabanero -f https://raw.githubusercontent.com/kabanero-io/kabanero-operator/0.2.0/config/samples/default.yaml`{{execute}}

Verify the installation and get the routes to the Kabanero landing and Tekton Dashboard pages

`oc get routes -n kabanero`{{execute}}

Wait that all pods are up and running

`watch oc get pods -n kabanero`{{execute}}

Open Tekton dashboard

``td=$(oc get routes tekton-dashboard -n kabanero -o jsonpath='{.spec.host}') http://$td:80``{{open}}


