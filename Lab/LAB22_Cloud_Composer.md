# Cloud Composer: Copying BigQuery Tables Across Different Locations

https://partner.cloudskillsboost.google/focuses/11579?locale=en&parent=catalog
https://cloud.google.com/blog/products/data-analytics/how-to-transfer-bigquery-tables-between-locations-with-cloud-composer

## Task 1. Create Cloud Composer environment

Can take +20mins

## Task 2. Create Cloud Storage buckets

gsutil mb -l us gs://qwiklabs-gcp-01-21b918a6a49b-us # Multi-regions
gsutil mb -l eu gs://qwiklabs-gcp-01-21b918a6a49b-eu # Multi-regions

## Task 3. BigQuery destination dataset

bq mk --location=EU --dataset nyc_tlc_EU

## Task 4. Airflow and core concepts, a brief introduction

## Task 5. Defining the workflow

https://github.com/GoogleCloudPlatform/python-docs-samples/blob/16de3b44f1a9faeccca5bc1cd8a719afc104dc65/composer/workflows/bq_copy_across_locations.py

## Task 6. Viewing environment information

Note: Cloud Composer uses Cloud Storage to store Apache Airflow DAGs, also known as workflows. Each environment has an associated Cloud Storage bucket. Cloud Composer schedules only the DAGs in the Cloud Storage bucket
=> DAGs folder = gs://us-east1-composer-advanced--e0c27076-bucket/dags

### Creating a virtual environment
apt-get install -y virtualenv
python3 -m venv venv
source venv/bin/activate

## Task 7. Setting DAGs Cloud Storage bucket

DAGS_BUCKET=us-east1-composer-advanced--e0c27076-bucket  ## remove gs:// and dags/

## Task 8. Setting Airflow variables used by DAG

gcloud composer environments run <ENVIRONMENT_NAME> \
--location <LOCATION> variables -- \
set <KEY> <VALUE> ## location here is the location of the GKE instance

gcloud composer environments run composer-advanced-lab \
--location us-east1 variables -- \
set gcs_source_bucket gs://qwiklabs-gcp-01-21b918a6a49b-us 

gcloud composer environments run composer-advanced-lab \
--location us-east1 variables -- \
set gcs_dest_bucket gs://qwiklabs-gcp-01-21b918a6a49b-eu

gcloud composer environments run composer-advanced-lab \
--location us-east1 variables -- \
set table_list_file_path /home/airflow/gcs/dags/bq_copy_eu_to_us_sample.csv

### to check if variables have been well set 

gcloud composer environments run composer-advanced-lab \
    --location us-east1 variables -- \
    get gcs_source_bucket

## Task 9. Uploading the DAG and dependencies to Cloud Storage

### Copy the Google Cloud Python docs samples files 

cd ~
gsutil -m cp -r gs://spls/gsp283/python-docs-samples .

### Upload a copy of the third party hook and operator to the plugins folder of your Composer DAGs Cloud Storage bucket

gsutil cp -r python-docs-samples/third_party/apache-airflow/plugins/* gs://$DAGS_BUCKET/plugins

### upload the DAG and config file to the DAGs Cloud Storage bucket

gsutil cp python-docs-samples/composer/workflows/bq_copy_across_locations.py gs://$DAGS_BUCKET/dags
gsutil cp python-docs-samples/composer/workflows/bq_copy_eu_to_us_sample.csv gs://$DAGS_BUCKET/dags

## Task 10. Using the Airflow UI
