# Lab Dataflow

## create GCS bucket

gsutil mb gs://qwiklabs-gcp-00-4ab0c2e6b50d
gsutil ls -L -b gs://qwiklabs-gcp-00-4ab0c2e6b50d

## install pip and dataflow sdk

### pulls a Docker container and then opens up a command shell to run inside the container
docker run -it -e DEVSHELL_PROJECT_ID=$DEVSHELL_PROJECT_ID python:3.7 /bin/bash

pip install 'apache-beam[gcp]'
python -m apache_beam.examples.wordcount --output OUTPUT_FILE

ls -al
cat OUTPUT_FILE-00000-of-00001


BUCKET=gs://qwiklabs-gcp-00-4ab0c2e6b50d
python -m apache_beam.examples.wordcount --project $DEVSHELL_PROJECT_ID \
  --runner DataflowRunner \
  --staging_location $BUCKET/staging \
  --temp_location $BUCKET/temp \
  --output $BUCKET/results/output \
  --region us-west1