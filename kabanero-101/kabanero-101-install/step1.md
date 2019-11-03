Install Appsody (CLI).

## OCP Login

To login to the OpenShift cluster from the _Terminal_ run:

``oc login -u developer -p developer  [[HOST_SUBDOMAIN]]-8443-[[KATACODA_HOST]].environments.katacoda.com``{{execute}}

This will log you in using the credentials:

* **Username:** ``developer``
* **Password:** ``developer``

Use the same credentials to log into the web console.

## Task

Download the Appsody package for Ubuntu

`mkdir -p /tmp/appsody && cd /tmp/appsody && wget https://github.com/appsody/appsody/releases/download/0.4.9/appsody_0.4.9_amd64.deb`{{execute}}

Install the Appsody package

`sudo apt install -f ./appsody_0.4.9_amd64.deb`{{execute}}

Verify the installation

`appsody list`{{execute}}


