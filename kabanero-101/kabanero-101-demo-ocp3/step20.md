Kabanero and Appsody with Quarkus example

## (Optional) OCP Login

Initially you are logged in as cluster admin. To switch to developer from the _Terminal_ run:

``oc login -u developer -p developer  [[HOST_SUBDOMAIN]]-8443-[[KATACODA_HOST]].environments.katacoda.com``{{execute}}

This will log you in using the credentials:

* **Username:** ``developer``
* **Password:** ``developer``

Use the same credentials to log into the web console.

## Create Quarkus application

Use *quarkus* Appsody stack for creating a new application

`mkdir -p /tmp/examples/quarkus && cd /tmp/examples/quarkus && appsody init experimental/quarkus`{{execute}}

Check the project template

`tree .`{{execute}}

In SELinux environment is it necessary to allow the docker daemon the access of the mounted project directory:

`chcon -Rt svirt_sandbox_file_t /tmp/examples/quarkus`{{execute}}

Run the application. Appsody creates and executes docker container (for Katacoda: share the host network).

`appsody run --network host`{{execute}}

Open the main page

`http://localhost:8888`{{open}}

