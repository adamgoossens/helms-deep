# How the components deploy

`bootstrap-supervisor-cluster.yaml` uses a simple `include_tasks` loop to include all of the files
within the `tasks/bootstrap/` directory.

These are numbered and named sequentially, and they will be processed in that order.

For example, `00-olm.yaml` comes first, followed by `10-acm.yaml`, `11-acm-tower-access.yaml`, etc.

Broadly speaking, the following prefixes apply:

* `00 - 09`: any prep work before the first application goes on the cluster.
* `10 - 19`: anything to do with ACM.
* `20 - 29`: anything to do with SSO.
* `30 - 39`: anything to do with ACS.
* `40 - 49`: anything to do with Quay.

After that, it's alphabetical order.
