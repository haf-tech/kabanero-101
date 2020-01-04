Deploy Application with Pipeline - using a Webhook.

**This step does not work properly with Katacoda, due Internet routing issues**

## Target

Deploys an application from GitHub repository using a predefined Pipeline. The pipeline will be triggered after a Git commit, with the help of a Webhook.

Outcome
* Application deployed using a webhook triggered Pipeline 

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


## Preparation

To enable the execution of a pipeline after a Git commit is it necessary to connect the Tekton pipeline with Git using a Webhook.
The creation Webhook and connecting of Tekton instance with a Github repository will be triggered directly from Tekton.

Use the Tekton Dashboard to configure the Webhook

`td=$(oc get routes tekton-dashboard -n tekton-pipelines -o jsonpath='{.spec.host}') && echo "http://$td:80"`{{execute}}

* Select `Webhooks`
* Click the button `Add Webhook`
* Name: e.g. "demoexpress-webhook" 
* Repository URL: Github repository URL, e.g. https://github.com/haf-tech/k101-nodejs-express
* Access Token: use the GitHub PAT
* Namespace: Namespace containing the Pipelines, `kabanero`
* Pipeline: the pipeline which will be triggered, depends from tech stack, e.g. `nodejs-express-build-push-deploy-pipeline`
* Service Account: `kabanero-operator`
* Docker Registry: e.g. "image-registry.openshift-image-registry.svc:5000/demo-express"
* Create

After a short period is the Webhook created in the GitHub repository.
Sometimes is it helpful to `Redeliver` the webhook because the relevant resources were not ready during the WebHook creation in GitHub.

* Select in GitHub the repository
* `Settings` > `Webhooks`
* Select the new created Webhook
* at the bottom verify the status of the last delivery
* if negative, select the last try and press `Redeliver`


## Execute the pipeline via Webhook via Github commit

Make a change in the Git repository and push the commit.
This will trigger a Pipeline via the Webhook.

To check the Webhook execution

* open the GitHub repository
* Select `Settings`
* and in `Webhooks` the created one
* ...in the bottom is a listing of the sent webhooks.

Any item contains details about request and response. Also it possible to `redeliver` a webhook request. **Redeliver** the commit only if now PipelineRun is already executed, otherwise every re-delivery will execute a new PipelineRun.

Check the existing PipelineRuns

`oc get pipelinerun -n kabanero`{{execute}}

Monitor the execution using the Tekton Dashboard

`td=$(oc get routes tekton-dashboard -n tekton-pipelines -o jsonpath='{.spec.host}') && echo "http://$td:80"`{{execute}}

...and wait until the pipeline is successfully executed.

Afterwards is the application also available in kAppNav

`td=$(oc get routes kappnav-ui-service -n kappnav -o jsonpath='{.spec.host}') && echo "https://$td/kappnav-ui/"`{{execute}}

Use the kAppNav UI to see which k8s resources are belong to the application.

...and the application itself:

`td=$(oc get routes k101-nodejs-express -n demo-express -o jsonpath='{.spec.host}') && echo "http://$td/"`{{execute}}