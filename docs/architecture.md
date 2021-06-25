# HELMS DEEP Architecture

HELMS DEEP is a collection of utilities deployed on a single, central OpenShift 4.latest cluster.

The utilities deployed are:

* Advanced Cluster Management for Kubernetes
* Advanced Cluster Security for Kubernetes
* Red Hat Single Sign On
* Red Hat Quay

## Bootstrap playbook

The bootstrap playbook is responsible for configuring the central server with all required services.

It will also configure some integrations between them - for example, creating Single Sign On clients for
the central cluster and ACS.

See `bootstrap-supervisor-cluster.yaml` for details.

## Single Sign On and Group Sync

During bootstrap, SSO is deployed. A Client is created for the central cluster, and then RH SSO is installed
into the `OAuth/cluster` resource as an OIDC endpoint. This allows individuals to log in to the central
cluster using RH SSO credentials.

Groups are not automatically synchronised from SSO onto OpenShift. To achieve that the GroupSync operator is 
used, configured within the `helms-deep-sso` namespace. The GroupSync operator is maintained by the 
Red Hat CoP; for more detail, see [here](https://github.com/redhat-cop/group-sync-operator).

A user specifically for GroupSync is created at bootstrap time, and this user is granted minimal permissions
on the `helms-deep` realm: namely, just to query groups and users. The credential for this user is placed
into a `Secret` in the `helms-deep-sso` namespace, `helms-deep-group-sync-credentials`.

GroupSync is configured to synchronise from SSO once every 15 minutes; see the `GroupSync/keycloak-groupsync`
resource in the `helms-deep-sso` namespace.

## ACM Deployment

HELMS DEEP installs the ACM operator and then installs ACM on the cluster, enabling management of the hub
cluster. It also installs a `Subscription` to a GitHub repository that hosts some basic resources; see
`bootstrap-templates/acm/resources-subscription.yaml.j2` for an example. This is configurable by changing
the `acm.gitopsRepoUrl` variable in `group_vars/all/all.yml`.

## ACS Deployment

ACS is currently deployed using the helm chart. See the configuration values exposed in `acs` in `group_vars/all/all.yml`
for details on how this is configured.

Once configured, ACS is integrated into Single Sign On. A `KeycloakClient` is created for ACS, and the details of
this are configured in ACS using the associated API; see `tasks/bootstrap/30-acs.yaml` for details.

Cluster collector services are also deployed on the central cluster. An init bundle is created, then the Helm chart deployed.
See `tasks/acs/deploy-collector.yaml` for details.
