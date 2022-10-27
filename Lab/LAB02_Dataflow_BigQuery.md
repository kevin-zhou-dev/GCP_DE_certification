# LAB : Dataflow - BigQuery

Create dataset

```bash
bq --location=us-west1 mk taxirides 
```

Create table

```bash
bq --location=us-west1 mk \
--time_partitioning_field timestamp \
--schema ride_id:string,point_idx:integer,latitude:float,longitude:float,\
timestamp:timestamp,meter_reading:float,meter_increment:float,ride_status:string,\
passenger_count:integer -t taxirides.realtime
```

Create GCS bucket

```bash
gsutil mb -p qwiklabs-gcp-01-17a560d6ff27 -l US gs://qwiklabs-gcp-01-17a560d6ff27 
# -p PROJECT_ID
# -l LOCATION
```

Create dataflow pipeline from template

```bash
gcloud dataflow jobs run streaming-taxi-pipeline \
    --gcs-location gs://dataflow-templates/latest/PubSub_to_BigQuery\
    --region  us-west1\
    --staging-location gs://qwiklabs-gcp-01-17a560d6ff27/tmp/ \
    --parameters \
inputTopic=projects/pubsub-public-data/topics/taxirides-realtime,\
outputTableSpec=qwiklabs-gcp-01-17a560d6ff27:taxirides.realtime
```

Test by launching a query

```bash
bq query --use_legacy_sql=false \
'SELECT * FROM taxirides.realtime LIMIT 10'
```

Single quotes in SQL query need to be transformed in double quote !

```bash
bq query --use_legacy_sql=false \
'WITH streaming_data AS (
SELECT
  timestamp,
  TIMESTAMP_TRUNC(timestamp, HOUR, "UTC") AS hour,
  TIMESTAMP_TRUNC(timestamp, MINUTE, "UTC") AS minute,
  TIMESTAMP_TRUNC(timestamp, SECOND, "UTC") AS second,
  ride_id,
  latitude,
  longitude,
  meter_reading,
  ride_status,
  passenger_count
FROM
  taxirides.realtime
WHERE ride_status = "dropoff"
ORDER BY timestamp DESC
LIMIT 1000
)
SELECT
 ROW_NUMBER() OVER() AS dashboard_sort,
 minute,
 COUNT(DISTINCT ride_id) AS total_rides,
 SUM(meter_reading) AS total_revenue,
 SUM(passenger_count) AS total_passengers
FROM streaming_data
GROUP BY minute, timestamp'
```

List jobs in Dataflow

```bash
gcloud dataflow jobs list
```

Drain the job identified with its id

```bash
gcloud dataflow jobs drain JOB_ID
```
