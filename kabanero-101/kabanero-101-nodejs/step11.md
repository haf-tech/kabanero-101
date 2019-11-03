Install Appsody (CLI).

## (Optionial) OCP Login

Initially you are logged in as cluster admin. To switch to developer from the _Terminal_ run:

``oc login -u developer -p developer  [[HOST_SUBDOMAIN]]-8443-[[KATACODA_HOST]].environments.katacoda.com``{{execute}}

This will log you in using the credentials:

* **Username:** ``developer``
* **Password:** ``developer``

Use the same credentials to log into the web console.

## Create NodeJS application

Use *nodejs-express* Appsody stack for creating a new application

`mkdir -p examples/nodejs-express && appsody init nodejs-express`{{execute}}

Check the project template

`tree .`{{execute}}

Run the application. Appsody creates and executes docker container.

`appsody run`{{execute}}

Open the main page

`http://localhost:3000`{{open}}

