Build and prepare Deployment

## Target

Understand the build and deploy mechanism of Appsody.


## Build

In case the application is still running from the previous step. Cancel the process ``strg+c`` or ``appsody stop``{{execute}} in the application directory.

The build command from Appsody extracts the application into a local directory and creates the docker image. 

`cd ~/nodejs-express`{{execute}}

The build allows different parameters. It is advisable to set the Docker tag name, including a version number.

`appsody build -t demo-express/k101-nodejs-express:v0.1`{{execute}}

Verify the created docker image

`docker images | grep k101-nodejs-express`{{execute}}

You can also start the docker image and test the logic

`docker run -d -p 8080:3000 --name nodejsexpress-test demo-express/k101-nodejs-express:v0.1`{{execute}}

See that the docker container is now running

`docker ps | grep nodejsexpress-test`{{execute}}

Open the logs and see that the application is started

`docker logs -f nodejsexpress-test`{{exeucte}}

Open the browser
* Katacoda: ``http://[[HOST_SUBDOMAIN]]-8080-[[KATACODA_HOST]].environments.katacoda.com``{{open}}
* Local machine: `http://localhost:8080`{{open}}

Stop the docker container

`docker stop nodejsexpress-test`{{execute}}


## Extract

To get a better insight about the generated docker image is it possible to check the files and configuration before they will be used for the docker image.

`mkdir -p ~/extract && appsody extract --target-dir ~/extract/k101-nodejs-express`{{execute}}

Verify the Dockerfile

`~/extract/k101-nodejs-express/Dockerfile`{{open}}

See that the Dockerfile includes the general base dependencies first, before installing the user-app dependencies.

The application source code is in the directory ``user-app``

`cd ~/extract/k101-nodejs-express/user-app`{{execute}}

You can verify 
* the project template structure
* and the docker image definition 
in the Kabanero Collection: https://github.com/kabanero-io/collections/tree/master/incubator/nodejs-express

## References

* Kabanero Collections: https://github.com/kabanero-io/collections
