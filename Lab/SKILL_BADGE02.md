# SKILL BADGE 2 Perform Foundational Data, ML, and AI Tasks in Google Cloud

## Dataflow job : load data into BigQuery from Pub/Sub with Dataflow batch template

gsutil mb -l us-west1 gs://b9bf6860-73c7-42a9-8c45-938909e56e90
bq --location=us-west1 mk lab_181

gcloud dataflow jobs run batch-pipeline \
--gcs-location gs://dataflow-templates/latest/GCS_Text_to_BigQuery \
--region us-west1 \
--staging-location gs://b9bf6860-73c7-42a9-8c45-938909e56e90/temp \
--parameters \
javascriptTextTransformGcsPath=gs://cloud-training/gsp323/lab.js,\
JSONPath=gs://cloud-training/gsp323/lab.schema,\
javascriptTextTransformFunctionName=transform,\
outputTable=qwiklabs-gcp-01-642ff0ecffb8:lab_181.customers_276,\
inputFilePattern=gs://cloud-training/gsp323/lab.csv,\
bigQueryLoadingTemporaryDirectory=gs://b9bf6860-73c7-42a9-8c45-938909e56e90/temp

### documentation here : https://github.com/GoogleCloudPlatform/DataflowTemplates/blob/main/v1/src/main/java/com/google/cloud/teleport/templates/TextIOToBigQuery.java

## Dataproc job

gcloud config set dataproc/region us-east1
gcloud dataproc clusters create example-cluster --worker-boot-disk-size 500

hdfs dfs -cp gs://cloud-training/gsp323/data.txt /data.txt ### ssh in one of the cluster node

gcloud dataproc jobs submit spark --cluster example-cluster \
  --class org.apache.spark.examples.SparkPageRank \
  --jars file:///usr/lib/spark/examples/jars/spark-examples.jar \
  --max-failures-per-hour 1 \
  -- /data.txt

## Dataprep job

## Cloud Speech API

gcloud alpha services api-keys create --display-name=API_CLOUD_SPEECH_KEY
export API_KEY=$(gcloud alpha services api-keys get-key-string 7fa9e6aa-f56a-4c5f-8bd7-7dabea6252fa --format="value(keyString)")
export API_KEY="AIzaSyCLtsmOkj_2Iz-5qufdITgCwTAXA5qNpk4"
touch request.json
nano request.json

### to be put in request.json
```json
{
  "config": {
      "encoding":"FLAC",
      "languageCode": "en-US"
  },
  "audio": {
      "uri":"gs://cloud-training/gsp323/task4.flac"
  }
}
```

curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}"

curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}" > result.json

gsutil cp result.json gs://qwiklabs-gcp-01-642ff0ecffb8-marking/task4-gcs-177.result