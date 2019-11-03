Install Kabanero Foundation.

Kabanero CLI manages the Kabanero collections.

## Task

Clone the git repository with the installation scripts

`mkdir -p /tmp/kabanero-foundation && cd /tmp/kabanero-foundation && git clone https://github.com/kabanero-io/kabanero-foundation.git && cd kabanero-foundation/scripts`{{execute}}

Install Kabanero Foundation, with the following paramters
* Set the Subdomain
* Enable kAppNav

``ENABLE_KAPPNAV=yes openshift_master_default_subdomain=[[HOST_SUBDOMAIN]]-8443-[[KATACODA_HOST]].environments.katacoda.com ./install-kabanero-foundation.sh``{{execute}}

Verify the installation and get the routes to the Kabanero landing page

`oc get routes -n kabanero`{{execute}}


