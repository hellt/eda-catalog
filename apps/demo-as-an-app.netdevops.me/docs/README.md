# demo-as-an-app

This application demonstrates the flexibility and power of EDA's application management and its workflow engine. The app brings up a single workflow named "Install Demo" that a user can run in any available namespace.

The workflow input has a single parameter named "namespace" that allows the user to specify the namespace where the demo topology and its resources will be created. If the user does not provide a value for this parameter, the workflow will create the topology in the default `demo-as-an-app` namespace.

The namespace will be created by the workflow, then the Try EDA three node topology will be created in that freshly minted namespace, and finally, a Fabric and Virtual Network resources will be created to complete the demo setup.

> This app has been tested on 25.12 EDA release.

The `srlinux-ghcr-25.10.1` node profile is used in this demo and it is currently not configurable.

## Testman commands

Using the default demo namespace

List all edge interfaces

```
edactl -n demo-as-an-app testman get-edge-if all
```

Get edge interface e-1-3 on leaf1 and leaf2:

```
edactl -n demo-as-an-app testman get-edge-if interface leaf1-ethernet-1-3
edactl -n demo-as-an-app testman get-edge-if interface leaf2-ethernet-1-3
```

Write down leaf1 edge interface name and leaf2 edge interface IP address for the next step.

Ping between two edge interfaces (interfaces e-1-3 on leaf1 and leaf2). The source is the edge interface name on leaf1 and destination is the IP address of the edge interface on leaf2 as extracted in the previous step:

```bash
# don't forget to replace the interface IP address to match your setup
edactl -n demo-as-an-app testman ping eif-name eif-leaf1-ethernet-1-3-vlan-10 10.0.0.11
```

The ping report should show 100% success rate and 0% packet loss.

## Developer notes

Change workflow code, then build the workflow binary:

```bash
make build-workflows
```

package it into a container:

```bash
make package-workflows
```

and deploy the current version in your cluster (this assumes that the app oci image and workflow image are public or you have auth tokens for the cluster to pull them up):

```bash
eb --app demo_as_an_app deploy
```

Or all in one step:

```bash
make build-workflows && make package-workflows && eb --app demo_as_an_app deploy
```
