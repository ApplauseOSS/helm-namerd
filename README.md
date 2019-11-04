## helm-namerd Chart

[Namerd](https://github.com/linkerd/linkerd/tree/master/namerd) is a service that resolves logical names to concrete addresses. By having Linkerd use Namerd for name delegation, we reduce the load on service discovery backends and can use Namerd as a central place to control routing across a fleet of linkers.

Namerd supports pluggable modules for dtab storage, naming and service discovery, and servable interfaces. For example, Namerd can be configured to use ZooKeeper for storage, ZooKeeper Serversets for service discovery, and a long-poll thrift interface.

### Chart Details

This chart will do the following:

- Create a deployment that provisions namerd with a default configuration and three replicas for HA.
- Create necessary RBAC for namerd
- Create [namerctl](https://github.com/linkerd/namerctl) (utility for controlling namerd) with a default config

### Installing the Chart

There are 2 steps to install this helm chart:

First, install the CRD (custom resource definition):

`kubectl apply -f namerd-crd.yaml`

Next, to install the chart with the release name my-release:

`helm install --name my-release ./helm-namerd`

### Configuration

Configurable values are documented in the values.yaml.

Specify each parameter using the --set key=value[,key=value] argument to helm install.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

`helm install --name my-release -f values.yaml ./helm-namerd`
