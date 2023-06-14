# Source Repository For ControlPlane Blue

This repository contains all configurations needed to operate the
`controlplane-blue`.

It is bootstrapped from a template repository.

## Getting Started

Prepare an Upbound Space by following instructions
[here](https://github.com/upbound-demo/environments-instructions). If you'd like
to use Vault as your secret store for the Kubeconfig of the `ControlPlane`, make
sure you install and enable the [secret store feature](https://docs.crossplane.io/knowledge-base/integrations/vault-as-secret-store/#configure-vault-for-crossplane-integration) in your Upbound Space.

In your Upbound Space, create your `ControlPlane` pointing to this
repository.

Create a `ControlPlane` pointing to this repository.
```yaml
apiVersion: mxp.upbound.io/v1alpha1
kind: ControlPlane
metadata:
  name: controlplane-blue
spec:

  # Requires "up.yaml" to be present.
  source:

    # Can have "type: Image" as option in future.
    type: Git
    git:
      url: https://github.com/upbound-demo/controlplane-blue
      ref:
        branch: main

  # Publishes to an external secret store like Vault.
  publishConnectionDetailsTo:
    name: kubeconfig-controlplane-blue
    configRef:
      name: vault
    metadata:
      labels:
        environment: development
        team: backend
```

## Developing

This is a `ControlPlaneSource` repo, which means that it contains the source of
truth for configuring a control plane. If you'd like to share this with another
control plane, follow the instructions below:

1. Fork this repository.
1. Replace the instances of the IAM Role ARN with the one that's assigned to
   your new control plane, `arn:aws:iam::609897127049:role/icp-production-control-plane`
   * [`providers/aws/controllerconfig.yaml`](providers/aws/controllerconfig.yaml)
1. Replace the instances where the control plane name is mentioned, `controlplane-blue`
   * [`environment/secretstore.yaml`](environment/secretstore.yaml)
   * [`environment/environmentconfig.yaml`](environment/environmentconfig.yaml)
1. Make sure the image pull secret mentioned in `Provider` and `Configuration`
   manifests are replaced by the one that exists in your control plane 