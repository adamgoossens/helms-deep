# HELMS DEEP
## An integrated demonstration platform for OCP, ACM, ACS, Quay...

HELMS DEEP is designed to take an RHPDS-deployed cluster and perform the following tasks:
* Deploy RH ACM.
* Deploy RH ACS.
* Deploy RH Quay.
* Deploy RH SSO.
* Configure Single Sign On realms and clients.
* Configure the OCP cluster to authenticate to SSO.
* (Eventually) Add a number of users and groups to SSO for different personas, e.g. 'developer', 'infra admin', 'security admin', 'auditor'.

The core playbook is `bootstrap-supervisor-cluster.yaml`. This requires a number of input environment variables:
* `OPENSHIFT_API_URL` - self-explanatory.
* `OPENSHIFT_API_INSECURE_CERTS` - `true` if API has insecure certs.
* `OPENSHIFT_API_USERNAME`, `OPENSHIFT_API_PASSWORD` - self-explanatory.
