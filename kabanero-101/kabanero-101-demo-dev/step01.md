Prepare the environment and install Appsody (CLI) and oc cli.

## Target

Install Appsody on the RHEL/CentOS system using the rpm package. The result is a working Appsody instance.
Also the OpenShift CLI will be installed.

## Install Appsody

Download the Appsody package for RHEL

`mkdir -p /tmp/appsody && cd /tmp/appsody && wget https://github.com/appsody/appsody/releases/download/0.5.0/appsody_0.5.0_amd64.deb`{{execute}}

Install the Appsody package

`sudo apt -y install ./appsody_0.5.0_amd64.deb`{{execute}}

Verify the installation

`appsody list`{{execute}}

## Install oc

Install the oc cli from the OKD repository, instead OCP - to avoid the subscription configuration.

`mkdir -p /tmp/oc-cli && cd /tmp/oc-cli && curl -s -L https://github.com/openshift/origin/releases/download/v3.11.0/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz -O`{{execute}}

Extract the artifact
`tar -xf openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz && rm -rf openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz`{{execute}}

...and place a symbolic link
`cd /usr/bin && ln -s /tmp/oc-cli/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit/oc oc`{{execute}}
