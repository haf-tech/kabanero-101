Kabanero and Appsody with NodeJS example

## (Optional) OCP Login

Initially you are logged in as cluster admin. To switch to developer from the _Terminal_ run:

``oc login -u developer -p developer  [[HOST_SUBDOMAIN]]-8443-[[KATACODA_HOST]].environments.katacoda.com``{{execute}}

This will log you in using the credentials:

* **Username:** ``developer``
* **Password:** ``developer``

Use the same credentials to log into the web console.

## Create NodeJS application

Use *nodejs-express* Appsody stack for creating a new application

`mkdir -p /tmp/examples/nodejs-express && cd /tmp/examples/nodejs-express && appsody init nodejs-express`{{execute}}

Check the project template

`tree .`{{execute}}

In SELinux environment is it necessary to allow the docker daemon the access of the mounted project directory:

`chcon -Rt svirt_sandbox_file_t /tmp/examples/nodejs-express`{{execute}}

Run the application. Appsody creates and executes docker container.

`appsody run`{{execute}}

Open the main page

`http://localhost:3000`{{open}}

