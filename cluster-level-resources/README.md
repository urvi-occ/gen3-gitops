# Cluster Level Resources
This helm chart covers all the __cluster level__ resources you might need or want to run a Gen3 instance. It follows the [app of apps](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/#helm-example) pattern to allow you to bootstrap all the resources you might need in one go. All of the values files must be available on a Git repo available to your ArgoCD instance, which you will set in your values file.

## Managing Values
While we are not opinionated on where your config lives, the Helm chart does assume a certain structure for your configuration that must be followed. The first assumption is that all of the values.yaml files you might use live in a directory whose name matches the `environment` field in values.yaml. The second is that all of the values files for this chart live under `environment/cluster-values`. The final is that each values file matches the name of the app it is configuring. (For example, if configuration is enabled for the `karpenter` app, then the chart expects that file to be named `karpenter.yaml`)

## Values Example
That may have been confusing, so let's go over an example to explain how to structure your configuration repo. Let's assume that your `values.yaml` for this chart looks like this:
```
cluster: my-awesome-cluster

configuration:
  configurationRepo: https://github.com/my-org/my-awesome-gitops
  configurationRevision: master

karpenter:
  enabled: true
  configuration:
    enabled: false

karpenter-templates:
  configuration:
    enabled: true
```

In this example, Karpenter is enabled, but configuration is not set, so it will just use the default values, and our karpenter-templates do have configuration enabled. Given this values file, the directory structure for the configuration repo should look like this:

```
.
├── less-cool-cluster
├── my-awesome-cluster
│   └── karpenter-templates.yaml
└── other-cluster
```
