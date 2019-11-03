Install Appsody (CLI).

## Create NodeJS application

Use *nodejs-express* Appsody stack for creating a new application

`mkdir -p examples/nodejs-express && appsody init nodejs-express`{{execute}}

Check the project template

`tree .`{{execute}}

Run the application. Appsody creates and executes docker container.

`appsody run`{{execute}}

Open the main page

`http://localhost:3000`{{open}}

