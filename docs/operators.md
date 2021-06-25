# Operator Subscriptions

You can change the operators that are deployed on the cluster, e.g. to add extra ones such as OSSM.

See `tasks/bootstrap/00-olm.yaml` for the logic that deploys `Namespaces`, `CatalogSources` and `Subscriptions`.

Within `group_vars/all/all.yml` you can modify the following:

* `namespaces` is a list that contains details on namespaces to be created during bootstrap. Use the
  existing ones as a template. All namespaces will, by default, have an `OperatorGroup` created in them
  automatically. If you don't want this done, pass `createOperatorGroup: false`. See the `stackrox`
  namespace for an example of that.
* `catalogSources` specifies which `CatalogSource` objects will be created. You can add additional ones
  here to capture other sources, such as `RedHatGov`.
* `subscriptions` is where you specify the Operators to subscribe to. Valid keys are `name`, `namespace`,
  `channel`, `startingCSV`, `source` and `installPlanApproval`. Only `channel`, `namespace` and `name` are required.

Please see the templates in `bootstrap-templates/olm/olm.yaml.j2` and `bootstrap-templates/olm/subscriptions.yaml.j2`.

We generate all of the necessary resources using Jinja then apply them at one time; this is significantly faster
than looping over each of them and deploying them individually.
