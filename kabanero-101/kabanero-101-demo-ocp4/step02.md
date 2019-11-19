Install Kabanero Foundation.

## Target

Installs Kabanero Foundation in the OCP cluster. The Kabanero Foundation contains all relevant cluster enhancement to enable Kabanero development and deployment.

# (Optional) OCP Login

Initially you are logged in as cluster admin. To switch to developer from the _Terminal_ run:

``oc login -u developer -p developer  [[HOST_SUBDOMAIN]]-8443-[[KATACODA_HOST]].environments.katacoda.com``{{execute}}

This will log you in using the credentials:

* **Username:** ``developer``
* **Password:** ``developer``

Use the same credentials to log into the web console.

For the case admin permissions needed, use

* **Username:** ``admin``
* **Password:** ``admin``

OCP Web console: ``https://console-openshift-console-[[HOST_SUBDOMAIN]]-443-[[KATACODA_HOST]].environments.katacoda.com``{{open}}


## Task

Retrieve install script for Kabanero Operator and install the operator/foundation resources, with enabling kAppNav:

`curl -s -L https://github.com/kabanero-io/kabanero-operator/releases/download/0.3.0-rc.4/install.sh -O && chmod +x install.sh`{{execute}}

`ENABLE_KAPPNAV=yes ./install.sh`{{execute}}

This will take a time until all resources are successfully deployed (approx 15min).

Install the Kabanero Custom Resource, but beforehand add the target namespace 

`wget https://raw.githubusercontent.com/kabanero-io/kabanero-operator/0.3.0-rc.4/config/samples/default.yaml`{{execute}}

```yaml
apiVersion: kabanero.io/v1alpha1
kind: Kabanero
metadata:
  name: kabanero
spec:
  version: "0.3.0"
  targetNamespaces:
  - demo-express
  collections: 
    repositories: 
    - name: central
      url: https://github.com/kabanero-io/collections/releases/download/0.3.0-rc.4/kabanero-index.yaml
      activateDefaultCollections: true

```
`oc apply -n kabanero -f default.yaml`{{execute}}

Verify the installation and get the routes to the Kabanero landing and Tekton Dashboard pages

`oc get pods -n kabanero -w`{{execute}}

`oc get routes -n kabanero`{{execute}}

Wait that all pods are up and running

`watch oc get pods -n kabanero`{{execute}}

## Get URLs

Open Tekton dashboard

`td=$(oc get routes tekton-dashboard -n tekton-pipelines -o jsonpath='{.spec.host}') && echo "http://$td:80"`{{execute}}

Open kAppNav UI - needs some time until the pods are ready

`oc get pods -n kappnav`{{execute}}

`td=$(oc get routes kappnav-ui-service -n kappnav -o jsonpath='{.spec.host}') && echo "https://$td/kappnav-ui/"`{{execute}}

