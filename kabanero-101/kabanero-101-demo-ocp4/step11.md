Kabanero and Appsody with NodeJS example

## Target

Create and test a new `nodejs-express` application using an Appsody stack.

## (Optional) OCP Login

Initially you are logged in as cluster admin. To switch to developer from the _Terminal_ run:

``oc login -u developer -p developer  [[HOST_SUBDOMAIN]]-8443-[[KATACODA_HOST]].environments.katacoda.com``{{execute}}

This will log you in using the credentials:

* **Username:** ``developer``
* **Password:** ``developer``

Use the same credentials to log into the web console.

## Create NodeJS application

Use *nodejs-express* Appsody stack for creating a new application

`mkdir -p ~/nodejs-express && cd ~/nodejs-express && appsody init kabanero/nodejs-express`{{execute}}

Check the project template

`tree .`{{execute}}

In SELinux environment is it necessary to allow the docker daemon the access of the mounted project directory:

`chcon -Rt svirt_sandbox_file_t ~/nodejs-express`{{execute}}

Run the application. Appsody creates and executes docker container (for Katacoda: share the host network).

`appsody run --network host`{{execute}}

Open the main page

Katacoda: ``http://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com``{{open}}
Local machine: `http://localhost:3000`{{open}}

## Modify application code

We will change the source code and any modification will be immediately available.

Open the app.js
`~/nodejs-express/app.js`{{open}}

Add the new endpoint with a random sleep
```javascript
const sleep = (waitTimeInMs) => new Promise(resolve => setTimeout(resolve, waitTimeInMs));

app.get('/echo/:val', (req, res) => {
  let val = req.params.val;

  let delay = Math.floor(1000 * (Math.random() * 5)); 
  sleep(delay).then(() => {
    res.send("Echo: " + val + "; delay=" + delay);
  })
  
});
```

Call the new URL (in a new browser)
Katacoda: ``http://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com/echo/a-value``{{open}}
Local machine: `http://localhost:3000/echo/a-value`{{open}}
