# Vals Operator Demo

## Instructions (for GKE)

Install Vals Operator in GKE:

```sh

helm repo add digitalis https://digitalis-io.github.io/helm-charts

kubectl create ns vals-operator

kubectl create secret generic -n vals-operator <CREDENTIALS_NAME> \
  --from-file=credentials.json=./gke/service_account.json

helm upgrade --install vals-operator --create-namespace -n vals-operator \
--set "env[0].name=GOOGLE_APPLICATION_CREDENTIALS,env[0].value=<GCP_APPLICATION_CREDENTIALS>" \
--set "env[1].name=GCP_PROJECT,env[1].value=<GCP_PROJECT_NAME>" \
--set "volumes[0].name=creds,volumes[0].secret.secretName=google-creds" \
--set "volumeMounts[0].name=creds,volumeMounts[0].mountPath=/secret" \
digitalis/vals-operator

```