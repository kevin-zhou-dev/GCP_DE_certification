# Lab Natural Language API

## create API key

export GOOGLE_CLOUD_PROJECT=$(gcloud config get-value core/project)
gcloud iam service-accounts create my-natlang-sa \
  --display-name "my natural language service account"
gcloud iam service-accounts keys create ~/key.json \
  --iam-account my-natlang-sa@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com
export GOOGLE_APPLICATION_CREDENTIALS="/home/USER/key.json"

gcloud compute instances create example-vm-instance

```
Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-04-762872b69668/zones/europe-west1-b/instances/example-vm-instance].
NAME: example-vm-instance
ZONE: europe-west1-b
MACHINE_TYPE: n1-standard-1
PREEMPTIBLE:
INTERNAL_IP: 10.132.0.2
EXTERNAL_IP: 34.79.66.163
STATUS: RUNNING
```

gcloud ml language analyze-entities --content="Michelangelo Caravaggio, Italian painter, is known for 'The Calling of Saint Matthew'." > result.json

cat result.json
