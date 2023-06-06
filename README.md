# Source Repository For ControlPlane Blue

This repository contains all configurations needed to operate the
`controlplane-blue`.

It is bootstrapped from a template repository.

## Getting Started

In you Upbound Environment, create your `ControlPlane` pointing to this
repository.


1. Enable the alpha source sync feature by passing the following flag
   during your Upobund Enviornments setup.
   ```bash
   helm -n upbound-system upgrade --install mxp oci://us-west1-docker.pkg.dev/orchestration-build/upbound-environments/mxp --version v0.9.3 --wait \
     --set "controlPlanes.enableSourceSync=true"
   ```

1. Create a `ControlPlane` pointing to this repository.
   ```yaml
   apiVersion: mxp.upbound.io/v1alpha1
   kind: ControlPlane
   metadata:
     name: controlplane-blue
   spec:
     source: # Alpha feature enabled by a flag.
       type: Git
       git:
         url: https://github.com/upbound-demo/controlplane-blue
       ref:
         branch: main
     backup:
       schedule: '* * * * 15'
     writeConnectionSecretToRef:
       name: kubeconfig-dev
       namespace: upbound-system
   ```