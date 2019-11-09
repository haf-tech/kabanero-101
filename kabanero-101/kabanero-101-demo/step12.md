Kabanero and Appsody with NodeJS example

## (Optional) OCP Login

Initially you are logged in as cluster admin. To switch to developer from the _Terminal_ run:

``oc login -u developer -p developer  [[HOST_SUBDOMAIN]]-8443-[[KATACODA_HOST]].environments.katacoda.com``{{execute}}

This will log you in using the credentials:

* **Username:** ``developer``
* **Password:** ``developer``

Use the same credentials to log into the web console.

## Build and generate deployment manifest

Build the docker image

`cd ~/nodejs-express && appsody build -t nodejs-express-simple:v0.1`{{execute}}

The deployment manifest is the definition for the Appsody operator.

`cd ~/nodejs-express && appsody deploy --generate-only -t nodejs-express-simple:v0.1`{{execute}}

Check the manifest file (e.g. the reference to the image)

`cat ~/nodejs-express/app-deploy.yaml`{{execute}}

## Deploy

Create the namespace/project

`oc new-project demo-express`{{execute}}

Deploy the app using Appsody operator 

`cd ~/nodejs-express && appsody deploy -t nodejs-express-simple:v0.1 --namespace demo-express`{{execute}}

