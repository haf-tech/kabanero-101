Push and deploy manually

## Target

Push the created image into a OCP Registry and deploy the application.

## Login

Stop the application if still running from the previous step - ``strg+c`` or `appsody stop`{{execute}}.

Get the oc cli command line with token from the OCP Console to login to the cluster

OCP Login
`oc login --token=ABC...Token --server=https://12344567-6443-shadow04.environments.katacoda.com`

Docker Login with the OCP Registry route
`docker login -u $(oc whoami) -p $(oc whoami -t) https://docker-registry-default.example.com`

Consider to replace the Registry URL with the right URL from your OpenShift cluster. To retrieve the URL

* in OCPv4: `oc get route -n openshift-image-registry -o jsonpath='{.spec.host}'`
* in OCPv3: `oc get route -n defautl -o jsonpath='{.spec.host}'`

If the route does not exist, create it with `passthrough`
`oc create route passthrough --service=image-registry`

To allow the access to an insecure docker registry, add the domain to the docker configuration on the client side:

`vi /etc/docker/daemon.json`{{execute}}

Example of the adjusted json file


```javascript
{
    "bip":"172.18.0.1/24",
    "debug": true,
    "storage-driver": "overlay",
    "insecure-registries": ["registry.test.training.katacoda.com:4567", "image-registry-openshift-image-registry.2886795280-80-shadow04.environments.katacoda.com"]
}
```

Restart the docker service

`systemctl restart docker.service`{{execute}}


docker login -u $(oc whoami) -p $(oc whoami -t) http://image-registry-openshift-image-registry.2886795280-80-shadow04.environments.katacoda.com


## Push

The build command does not push the image automatically to a Container Registry. To do so use the `--push` or the `--push-url` flag. E.g. to push to OpenShift Registry

`appsody build -t demo-express/k101-nodejs-express:v0.1 --push-url docker-registry-default.example.com`{{execute}}

Consider to replace the Registry URL with the right URL from your OpenShift cluster. To retrieve the URL

* in OCPv4: `oc get route -n openshift-image-registry -o jsonpath='{.spec.host}'`
* in OCPv3: `oc get route -n defautl -o jsonpath='{.spec.host}'`

Verify in the OCP cluster if the push created a new ImageStream in the project `demo-express`

`oc get is -n demo-express`{{execute}}

## Deploy directly into OCP Cluster

Appsody allows a direct deployment into a k8s/OCP cluster.
For this is it mandatory that a current session to the OCP Cluster exists.

OCP Login
`oc login -u ... -p --server=https://consol.master.example.com`

Docker Login
`docker login -u $(oc whoami) -p $(oc whoami -t) https://docker-registry-default.example.com`

And deploy the application to a target namespace

`appsody deploy -t demo-express/k101-nodejs-express:v0.1 -n demo-express --push-url=docker-registry-default.example.com`{{execute}}

The logic builds the docker image, generates the manifest file (`app-deploy.yaml`) if not already available and deploy it using in general the Appsody operator into the cluster. The image will also be pushed to the OpenShift registry which is mandatory, so that the application/POD can pull the image.

`oc policy add-role-to-user system:image-puller system:serviceaccount:kabanero:k101-nodejs-express --namespace=demo-express2`



