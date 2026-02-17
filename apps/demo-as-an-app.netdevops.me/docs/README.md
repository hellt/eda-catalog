# demo-as-an-app

Change workflow code, then build the workflow binary:

```bash
make build-workflows
```

then package it into a container:

```bash
make package-workflows
```

and deploy the current version in your cluster (this assumes that the app oci image and workflow image are public or you have auth tokens for the cluster to pull them up):

```bash
eb --app demo_as_an_app deploy
```
