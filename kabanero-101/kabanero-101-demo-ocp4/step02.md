Deploy Application with Pipeline.

## Target

Deploys an application from GitHub repository using a predefined Pipeline. The pipeline will be executed manually.

Outcome
* Application deployed using a manually trigger Pipeline 

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


## Overview

For the demo an application on the base of nodejs-express will be used. For this reason is the predefined Pipeline ``nodejs-express-build-push-deploy-pipeline`` relevant.
See the definition of the pipeline and determines the referenced tasks

`oc get pipeline -n kabanero nodejs-express-build-push-deploy-pipeline -o yaml`{{execute}}

The Pipeline definition reference the following tasks
* build-push-task => nodejs-express-build-push-task
* image-scan-task => nodejs-express-image-scan-task
* deploy-task => nodejs-express-deploy-task

The last two tasks will be executed in parallel.

The used tasks:

`oc get task -n kabanero nodejs-express-build-push-task -o yaml`{{execute}}

This extract the assemble, validates if the collection is available before building and pushing the new created docker image.
Consider that two input parameter are used, referenced as ``PipelineResource``, named ``git-source`` and ``docker-image``.

`oc get task -n kabanero nodejs-express-image-scan-task -o yaml`{{execute}}

This pulls the new created image and scans it.

`oc get task -n kabanero nodejs-express-deploy-task -o yaml`{{execute}}

The last task handles the deployment of a nodejs-express application. The main part is the application of the ``app-deploy.yaml``.

## Preparation

To execute the pipeline is it necessary to fulfil the requirements and create the needed PipelineResources for git-source and docker-image.

Set some variables to our needs
```
namespace=kabanero
APP_REPO=https://github.com/haf-tech/k101-nodejs-express.git
REPO_BRANCH=kabanero-run
DOCKER_IMAGE="image-registry.openshift-image-registry.svc:5000/demo-express/k101-nodejs-express:v0.1"
```

And generate the PipelineResource and apply them
```
cat <<EOF | oc -n ${namespace} apply -f -
apiVersion: v1
items:
- apiVersion: tekton.dev/v1alpha1
  kind: PipelineResource
  metadata:
    name: docker-image
  spec:
    params:
    - name: url
      value: ${DOCKER_IMAGE}
    type: image
- apiVersion: tekton.dev/v1alpha1
  kind: PipelineResource
  metadata:
    name: git-source
  spec:
    params:
    - name: revision
      value: ${REPO_BRANCH}
    - name: url
      value: ${APP_REPO}
    type: git
kind: List
EOF
```

Afterwards we have two resources for the pipeline

`oc get pipelineresource -n kabanero`{{execute}}

To access the GitHub repository is it needed to have a Secret with the GitHub PAT (Personal Access Token - [HowTo](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line), with `admin:repo_hook, repo`). 
Add the Secret using the Tekton Dashboard

`td=$(oc get routes tekton-dashboard -n tekton-pipelines -o jsonpath='{.spec.host}') && echo "http://$td:80"`{{execute}}

1. Select ``Secrets``
1. Click button ``Add Secret``
1. Fill out the wizard:
 1. Name: e.g. "github-pat"
 1. Namespace: ``Kabanero``
 1. Access To: ``Git Srever``
 1. Username: your Git username
 1. Password/Token: your PAT
 1. Service Account: `kabanero-operator`
 1. Server URL: ``https://github.com``
1. Submit


## Pre-Verification

Verify the configuration using the Tekton Dashboard:


`td=$(oc get routes tekton-dashboard -n tekton-pipelines -o jsonpath='{.spec.host}') && echo "http://$td:80"`{{execute}}

Go through the dashboard and check
* the existing Pipelines
* ...the definition of the relevant Pipeline
* ...and the referenced Tasks
* also if PipelineRuns exists (and the history)
* also the PipelineResource

## Execute manually the pipeline

To trigger a Pipeline a ``PipelineRun`` will be applied which reference the wanted Pipeline.
Consider to change here `APP_REPO` if you are using another Git repo, also the `REPO_BRANCH`.

Set (again) the needed information
```
namespace=kabanero
APP_REPO=https://github.com/haf-tech/k101-nodejs-express.git
REPO_BRANCH=kabanero-run
DOCKER_IMAGE="image-registry.openshift-image-registry.svc:5000/demo-express/k101-nodejs-express:v0.1"
```

Create and execute the PipelineRun definition (needs approx 25min).
```
cat <<EOF | oc -n ${namespace} apply -f -
apiVersion: tekton.dev/v1alpha1
kind: PipelineRun
metadata:
  name: nodejs-express-build-deploy-pipeline-run-3
  namespace: kabanero
spec:
  pipelineRef:
    name: nodejs-express-build-push-deploy-pipeline
  resources:
  - name: git-source
    resourceRef:
      name: git-source
  - name: docker-image
    resourceRef:
      name: docker-image
  serviceAccount: kabanero-operator
  timeout: 60m
EOF
```

Check the existing PipelineRuns

`oc get pipelinerun -n kabanero`{{execute}}

Monitor the execution using the Tekton Dashboard

`td=$(oc get routes tekton-dashboard -n tekton-pipelines -o jsonpath='{.spec.host}') && echo "http://$td:80"`{{execute}}

...and wait until the pipeline is successfully executed.

It is also possible to monitor the logs directly from the container. The PipelineRun is a POD in ``kabanero`` namespace, containing different containers representing a step.

`oc get pods -n kabanero | grep nodejs-express | awk {'print $1'}`{{execute}}

Dependent in which phase the command is executed 1 to 3 pods will be returned, cause the PipelineRun has 3 tasks.
Retrieve the logs from the pod and select the container/step
`oc logs -f <pod-name> -c <container-name>`

You will receive the container names, if you call the command without the ``-c`` flag.


Afterwards is the application also available in kAppNav

`td=$(oc get routes kappnav-ui-service -n kappnav -o jsonpath='{.spec.host}') && echo "https://$td/kappnav-ui/"`{{execute}}

Use the kAppNav UI to see which k8s resources are belong to the application.

Also the application is deployed in the target namespace ``demo-express``.

`oc get pods, route -n demo-express`{{execute}}

...with a predefined URL:

`td=$(oc get routes k101-nodejs-express -n demo-express -o jsonpath='{.spec.host}') && echo "http://$td/"`{{execute}}