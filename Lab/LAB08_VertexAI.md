# Lab : Run a vertex ai pipeline stored in GCS

## Prepare script in GCS

gsutil mb gs://qwiklabs-gcp-01-7ca990f34890
touch emptyfile1
touch emptyfile2
gsutil cp emptyfile1 gs://qwiklabs-gcp-01-7ca990f34890/pipeline-output/emptyfile1
gsutil cp emptyfile2 gs://qwiklabs-gcp-01-7ca990f34890/pipeline-input/emptyfile2

wget https://storage.googleapis.com/cloud-training/dataengineering/lab_assets/ai_pipelines/basic_pipeline.json

sed -i 's/PROJECT_ID/qwiklabs-gcp-01-7ca990f34890/g' basic_pipeline.json

gsutil cp basic_pipeline.json gs://qwiklabs-gcp-01-7ca990f34890/pipeline-input/basic_pipeline.json

## Run a pipeline from vertex ai with file from GCS
