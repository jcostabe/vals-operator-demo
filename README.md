# Vals Operator Demo

1. [Prerequisites](#prerequisites)
2. [Instructions (for GKE)](#instructions-for-gke)

## Prerequisites

- Create two secrets in Secret Manager

## Instructions (for GKE)

Add Helm repository

```sh
helm repo add digitalis https://digitalis-io.github.io/helm-charts
```

Create namspace for Vals Operator objects:

```sh
kubectl create ns vals-operator
```

Create secret with GCP Credentials needed to access Secret Manager's objects

```sh
kubectl create secret generic -n vals-operator google-creds \
  --from-file=credentials.json=./gke/service_account.json
```

Deploy Vals Operator in GKE:

```sh
helm upgrade --install vals-operator --create-namespace -n vals-operator \
--set "env[0].name=GOOGLE_APPLICATION_CREDENTIALS,env[0].value=<GCP_APPLICATION_CREDENTIALS>" \
--set "env[1].name=GCP_PROJECT,env[1].value=<GCP_PROJECT_NAME>" \
--set "volumes[0].name=creds,volumes[0].secret.secretName=google-creds" \
--set "volumeMounts[0].name=creds,volumeMounts[0].mountPath=/secret" \
digitalis/vals-operator
```