Build and prepare Deployment

## Target

Understand the build and deploy mechanism of Appsody.


## Build

In case the application is still running from the previous step. Cancel the process ``strg+c`` or ``appsody stop``{{execute}} in the application directory.

The build command from Appsody extracts the application into a local directory and creates the docker image. 

`cd ~/nodejs-express`{{execute}}

The build allows different parameters. It is advisable to set the Docker tag name, including a version number.

`appsody build -t demo-express/k101-nodejs-express:v0.1`{{execute}}

Verify the created docker image

`docker images | grep k101-nodejs-express`{{execute}}

You can also start the docker image and test the logic

`docker run -d -p 8080:3000 --name nodejsexpress-test demo-express/k101-nodejs-express:v0.1`{{execute}}

See that the docker container is now running

`docker ps | grep nodejsexpress-test`{{execute}}

Open the logs and see that the application is started

`docker logs -f nodejsexpress-test`{{exeucte}}

Open the browser
* Katacoda: ``http://[[HOST_SUBDOMAIN]]-8080-[[KATACODA_HOST]].environments.katacoda.com``{{open}}
* Local machine: `http://localhost:8080`{{open}}


## Extract

To get a better insight about the generated docker image is it possible to check the files and configuration before they will be used for the docker image.

`mkdir -p ~/extract && appsody extract --target-dir ~/extract/k101-nodejs-express`{{execute}}

Verify the Dockerfile

`~/extract/k101-nodejs-express/Dockerfile`{{open}}

See that the Dockerfile includes the general base dependencies first, before installing the user-app dependencies.

The application source code is in the directory ``user-app``

`cd ~/extract/k101-nodejs-express/user-app`{{execute}}


## Push

Stop the application if still running from the previous step - ``strg+c`` or `appsody stop`{{execute}}.

Get the oc cli command line with token from the OCP Console to login to the cluster

OCP Login
`oc login --token=ABC...Token --server=https://12344567-6443-shadow04.environments.katacoda.com`

Docker Login
`docker login -u $(oc whoami) -p $(oc whoami -t) https://docker-registry-default.example.com`

oc expose svc image-registry
oc get route

vi /etc/docker/daemon.json
systemctl restart docker.service

----
{
    "bip":"172.18.0.1/24",
    "debug": true,
    "storage-driver": "overlay",
    "insecure-registries": ["registry.test.training.katacoda.com:4567", "image-registry-openshift-image-registry.2886795280-80-shadow04.environments.katacoda.com"]
}
----


docker login -u $(oc whoami) -p $(oc whoami -t) http://image-registry-openshift-image-registry.2886795280-80-shadow04.environments.katacoda.com



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






--

Use *nodejs-express* Appsody stack from the Kabanero Collection/Repository for creating a new application

`mkdir -p ~/nodejs-express && cd ~/nodejs-express && appsody init kabanero/nodejs-express`{{execute}}

Check the project template

`tree .`{{execute}}

In SELinux environment is it necessary to allow the docker daemon the access of the mounted project directory:

`chcon -Rt svirt_sandbox_file_t ~/nodejs-express`{{execute}}

Run the application. Appsody creates and executes docker container (for Katacoda: share the host network).

`appsody run --network host`{{execute}}

Open the main page

Katacoda: ``http://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com``{{open}}
Local machine: `http://localhost:3000`{{open}}

## Modify application code

We will change the source code and any modification will be immediately available.

Open the app.js
`~/nodejs-express/app.js`{{open}}

Add the new endpoint with a random sleep
```javascript
const sleep = (waitTimeInMs) => new Promise(resolve => setTimeout(resolve, waitTimeInMs));

app.get('/echo/:val', (req, res) => {
  let val = req.params.val;

  let delay = Math.floor(1000 * (Math.random() * 5)); 
  sleep(delay).then(() => {
    res.send("Echo: " + val + "; delay=" + delay);
  })
  
});
```

Call the new URL (in a new browser)
Katacoda: ``http://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com/echo/a-value``{{open}}
Local machine: `http://localhost:3000/echo/a-value`{{open}}

You will figure out, that any change will be immediately applied. Appsody monitors the filesystem and restarts the server to apply the changes. The docker container remains the same and this increase the time to apply new changes.