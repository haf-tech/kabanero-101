Install Appsody (CLI).

## Target

Install Appsody on the RHEL/CentOS system using the rpm package. The result is a working Appsody instance.

## (Optional) OCP Login

Initially you are logged in as cluster admin. To switch to developer from the _Terminal_ run:

``oc login -u developer -p developer  [[HOST_SUBDOMAIN]]-8443-[[KATACODA_HOST]].environments.katacoda.com``{{execute}}

This will log you in using the credentials:

* **Username:** ``developer``
* **Password:** ``developer``

Use the same credentials to log into the web console.

## Task RHEL/CentOS

Download the Appsody package for RHEL

`mkdir -p /tmp/appsody && cd /tmp/appsody && wget https://github.com/appsody/appsody/releases/download/0.4.10/appsody-0.4.10-1.x86_64.rpm`{{execute}}

Install the Appsody package

`sudo yum -y install ./appsody-0.4.10-1.x86_64.rpm`{{execute}}

Verify the installation

`appsody list`{{execute}}




