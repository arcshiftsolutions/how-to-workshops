# Getting Started Demo

> TODO: Update Support Links for the Service Definition and further how-tos and workshops

This demo is intended for BCGov Development Teams on the OpenShift Silver or Gold Service Clusters that have read through the Vault Service definition, and wish to confirm Vault is functioning as expected in their namespaces.

## Generating Deployment Manifests for Testing

1. Set Required Environemnt Variables

    ```bash
    export LICENSE_PLATE=<your license plate here>
    export CLUSTER_NAME=silver #gold, and golddr are also supported options
    ```

2. Generate Deployments

    Run the `create-manifests.sh` script to generate 4 deployment manifests in the same directory with the following names:

    `tools`, `dev`, and `test` will use Vaults `$LICENSE_PLATE-nonprod` Mount Point and Role.
    - vault-$LICENSE_PLATE-tools.yaml
    - vault-$LICENSE_PLATE-dev.yaml
    - vault-$LICENSE_PLATE-test.yaml

    `prod` will use Vaults `$LICENSE_PLATE-prod` Mount Point and Role.
    - vault-$LICENSE_PLATE-prod.yaml

3. Verify the words LICENSE, CLUSTER, MOUNT_ENV, and ENV have been replaced in the 4 files.

## Apply and Verify the Deployment Works in each Environment

  `oc apply -f vault-$LICENSE_PLATE-$ENV`

  > **Note:** It is recommended to start with the tools

  **Verify the Pod is up with `2/2` containers `READY`**

  ```console
  $ oc get pods
  NAME                         READY   STATUS    RESTARTS   AGE
  vault-test-7c6cd9f45f-n4dnt   2/2    Running   0          1d

  **Verify the Pod Logs are outputing `world`**

  ```console
  $ oc logs deployment/vault-test -c vault
  world


  world


  world


  world
  ```

  **View the Injected Secrets Template** (Optional)

  Grab the pod name from `oc get pods`

  ```console
  $ oc rsh -c vault vault-test-7c6cd9f45f-n4dnt 
  $ cat /vault/secrets/helloworld

  world

  ```

  **Clean Up!!** - Let's not waste log space

  `oc delete -f vault-$LICENSE_PLATE-$ENV`

  Assuming your logs look similar to those shown above your project set is configured and setup for integrating Vault with your Applications! :tada:

## Support

For information on Vault and for getting Vault support from the community check out [#devops-vault](https://chat.developer.gov.bc.ca/channel/devops-vault) on RocketChat!
