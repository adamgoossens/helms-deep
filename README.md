# HELMS DEEP

Note: not *actually* using Helm...for now :)

## An integrated demonstration platform for OCP, ACM, ACS, Quay...

HELMS DEEP is designed to take an RHPDS-deployed cluster and perform the following tasks:

* Deploy RH ACM.
* Deploy RH ACS.
* Deploy RH Quay.
* Deploy RH SSO.
* Configure Single Sign On realms and clients.
* Configure the OCP cluster to authenticate to SSO.
* (Eventually) Deploy a separate ELK stack to receive audit logs from satellite clusters and the supervisor cluster.
* (Eventually) Add a number of users and groups to SSO for different personas, e.g. 'developer', 'infra admin', 'security admin', 'auditor'.
* (Eventually) Configure log forwarding to push audit logs into the ELK stack.

The goal is to establish an integrated environment, across Dev, Sec and Ops, that can be used as a demonstration and storytelling platform. Ideally we should be able to adopt any given persona (e.g. "infrastructure admin", "developer", "security auditor") and tell a story around that persona across the integrated technologies.

The core playbook is `bootstrap-supervisor-cluster.yaml`. This requires a number of input environment variables:

* `OPENSHIFT_API_URL` - self-explanatory.
* `OPENSHIFT_API_INSECURE_CERTS` - `true` if API has insecure certs.
* `OPENSHIFT_API_USERNAME`, `OPENSHIFT_API_PASSWORD` - self-explanatory.

## Getting started

To start, provision a RHPDS OCP 4.7 workshop cluster - or deploy your own, that works too.

Once it's provisioned, run the `bootstrap-supervisor-cluster.yaml` playbook and provide the necessary environment variable inputs:

```
$ export OPENSHIFT_API_URL='<url_to_cluster_api>'
$ export OPENSHIFT_API_INSECURE_CERTS='true or false'
$ export OPENSHIFT_API_USERNAME='username'
$ export OPENSHIFT_API_PASSWORD='password'
$
$ ansible-playbook -i localhost, bootstrap-supervisor-cluster.yaml
```

Then sit tight and wait for completion!

## What needs doing?

- Deploy Ansible Automation Platform on the central cluster, then configure the necessary secrets in ACM for prehook/posthook.
- Deploy a Gitea instance on the cluster, and use that as the subscription source for ACM and the deployed AAP. This allows
  people to demonstrate the cluster and modify resources, without needing to change the upstream repositores.
- Build vignettes that can be told using the environment.

## TODO: Architectural Diagram.

