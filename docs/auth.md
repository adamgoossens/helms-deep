# How Kubernetes Authentication works

HELMS DEEP uses a simple included task, `tasks/k8s-auth.yaml` to perform authentication.

This task expects the following variables as input:

* `host` - API URL
* `username` - API username
* `password` - API password
* `validate_certs` - true or false.

It will also accept `token` as input, e.g. if you are using a long-lived token for authentication.

The task will generate a temporary kubeconfig file and make this available under the variable `kubeconfig.path`.

`bootstrap-supervisor-cluster` uses `module_defaults` to ensure that required k8s modules are automatically
given `kubeconfig.path` as part of their `kubeconfig` parameter. This means that we don't need to repeatedly
specify auth details for every call to `k8s_info`, `k8s`, `helm`, etc.
