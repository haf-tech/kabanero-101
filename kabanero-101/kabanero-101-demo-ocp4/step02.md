Install Kabanero Foundation.

## Target

Installs Kabanero Foundation in the OCP cluster. The Kabanero Foundation contains all relevant cluster enhancement to enable Kabanero development and deployment.

## Task

Retrieve install script for Kabanero Operator and install the operator/foundation resources, with enabling kAppNav:

`curl -s -L https://github.com/kabanero-io/kabanero-operator/releases/download/0.3.0-rc.3/install.sh -O && chmod +x install.sh`{{execute}}

`ENABLE_KAPPNAV=yes ./install.sh`{{execute}}

This will take a time until all resources are successfully deployed (approx 15min).

Install the Kabanero Custom Resource

`oc apply -n kabanero -f https://raw.githubusercontent.com/kabanero-io/kabanero-operator/0.3.0-rc.3/config/samples/default.yaml`{{execute}}

Verify the installation and get the routes to the Kabanero landing and Tekton Dashboard pages

`oc get routes -n kabanero`{{execute}}

Wait that all pods are up and running

`watch oc get pods -n kabanero`{{execute}}

## Get URLs

Open Tekton dashboard

`td=$(oc get routes tekton-dashboard -n tekton-pipelines -o jsonpath='{.spec.host}') && echo "http://$td:80"`{{execute}}

Open kAppNav UI - needs some time until the pods are ready

`oc get pods -n kappnav`{{execute}}

`td=$(oc get routes kappnav-ui-service -n kappnav -o jsonpath='{.spec.host}') && echo "https://$td/kappnav-ui/"`{{execute}}

