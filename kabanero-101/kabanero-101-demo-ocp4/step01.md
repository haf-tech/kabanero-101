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

For the case admin permissions needed, use

* **Username:** ``admin``
* **Password:** ``admin``

OCP Web console: ``https://console-openshift-console-[[HOST_SUBDOMAIN]]-443-[[KATACODA_HOST]].environments.katacoda.com``{{open}}

## Task RHEL/CentOS

Download the Appsody package for RHEL

`mkdir -p /tmp/appsody && cd /tmp/appsody && wget https://github.com/appsody/appsody/releases/download/0.5.0/appsody-0.5.0-1.x86_64.rpm`{{execute}}

Install the Appsody package

`yum -y install ./appsody-0.5.0-1.x86_64.rpm`{{execute}}

Verify the installation

`appsody list`{{execute}}

## Prepare CentOS 8

Install config-manager plugin

`dnf install -y 'dnf-command(config-manager)'`{{execute}}

Add Docker repo

`dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo`{{execute}}

Install docker

`dnf install -y docker-ce-3:18.09.1-3.el7`{{execute}}


