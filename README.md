## helm-namerd Chart

[Namerd](https://github.com/linkerd/linkerd/tree/master/namerd) is a service that resolves logical names to concrete addresses. By having Linkerd use Namerd for name delegation, we reduce the load on service discovery backends and can use Namerd as a central place to control routing across a fleet of linkers.

Namerd supports pluggable modules for dtab storage, naming and service discovery, and servable interfaces. For example, Namerd can be configured to use ZooKeeper for storage, ZooKeeper Serversets for service discovery, and a long-poll thrift interface.

### Chart Details

This chart will do the following:

- Create a deployment that provisions namerd with a default configuration and three replicas for HA.
- Create necessary RBAC for namerd
- Create [namerctl](https://github.com/linkerd/namerctl) (utility for controlling namerd) with a default config

### How does this chart differ from the stable/namerd helm chart?
This namerd helm chart adds the following:

- ability to override the ConfigMap to add the `io.l5d.rewrite` namer
- ability to add the `io.l5d.mesh` interface to the ConfigMap
- ability to expose the `mesh` and `admin` ports from the Deployment
- ability to override the command/args to add `taskset` to limit CPU count
- uses `CRD` instead of the removed `ThirdPartyResource` type
- `RBAC` to access CRDs
- ability to add `DTABs` and apply them automatically (currently done with a custom Job)

### Installing the Chart

There are 2 steps to install this helm chart:

First, install the CRD (custom resource definition):

`kubectl apply -f namerd-crd.yaml`

Next, to install the chart with the release name my-namerd:

`helm add repo <repo_name> https://applauseoss.github.io/helm-namerd/`

`helm repo update`

`helm install --name my-namerd <repo_name>/helm-namerd`

### Uninstalling the Chart

To uninstall this helm chart,

`helm delete --purge my-namerd`

### Configuration

Configurable values are documented in the values.yaml.

Specify each parameter using the `--set key=value[,key=value]` argument to helm install.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

`helm install --name my-namerd -f values.yaml <repo_name>/helm-namerd`

Supported parameters are:

| Parameter | Description | Default |
|----|----|----|
| `namerd.image.repo` | Namerd image registry | `buoyantio/namerd` |
| `namerd.image.tag` | Namerd image tag | `1.7.0` |
| `namerd.image.pullPolicy` | Image pull policy | `Always` |
| `namerd.podAnnotations` | Additional annotations for the namerd pod | `{}` |
| `namerd.replicas` | Number of replicas | `3` |
| `namerd.service.annotations` | Additional annotations for namerd service | `{}` |
| `namerd.service.type` | Kubernetes service type | `ClusterIP` |
