Install Appsody (CLI).

## Target

Install Appsody on the RHEL/CentOS system using the rpm package. The result is a working Appsody instance.


## Install Appsody

Download the Appsody package for RHEL

`mkdir -p /tmp/appsody && cd /tmp/appsody && wget https://github.com/appsody/appsody/releases/download/0.5.0/appsody_0.5.0_amd64.deb`{{execute}}

Install the Appsody package

`sudo apt -y install ./appsody_0.5.0_amd64.deb`{{execute}}

Verify the installation

`appsody list`{{execute}}

