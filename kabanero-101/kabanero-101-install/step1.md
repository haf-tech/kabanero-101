Install Appsody (CLI).

## Task

Download the Appsody package for Ubuntu

`mkdir -p /tmp/appsody && cd /tmp/appsody && wget https://github.com/appsody/appsody/releases/download/0.4.9/appsody_0.4.9_amd64.deb`{{execute}}

Install the Appsody package

`sudo apt install -f appsody_0.4.9_amd64.deb`{{execute}}

Verify the installation

`appsody list`{{execute}}


