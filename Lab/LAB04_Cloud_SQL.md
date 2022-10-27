# LAB : Create a Cloud SQL instance and populate it with data

List of account

```bash
gcloud auth list
```

List of project

```bash
gcloud config list project
export PROJECT_ID=$(gcloud info --format='value(config.project)')
export BUCKET=${PROJECT_ID}-ml
```

create cloud Sql instance

```bash
gcloud sql instances create taxi \
    --tier=db-n1-standard-1 --activation-policy=ALWAYS
```

Define password

```bash
gcloud sql users set-password root --host % --instance taxi \
 --password Passw0rd
```

Adress env of cloud shell IP adresss

```bash
export ADDRESS=$(wget -qO - http://ipecho.net/plain)/32
```

whitelist cloud shell instance

```bash
gcloud sql instances patch taxi --authorized-networks $ADDRESS
MYSQLIP=$(gcloud sql instances describe \
taxi --format="value(ipAddresses.ipAddress)")
mysql --host=$MYSQLIP --user=root \
      --password --verbose
```

```sql
  create database if not exists bts;
  use bts;
  drop table if exists trips;
  create table trips (
    vendor_id VARCHAR(16),
    pickup_datetime DATETIME,
    dropoff_datetime DATETIME,
    passenger_count INT,
    trip_distance FLOAT,
    rate_code VARCHAR(16),
    store_and_fwd_flag VARCHAR(16),
    payment_type VARCHAR(16),
    fare_amount FLOAT,
    extra FLOAT,
    mta_tax FLOAT,
    tip_amount FLOAT,
    tolls_amount FLOAT,
    imp_surcharge FLOAT,
    total_amount FLOAT,
    pickup_location_id VARCHAR(16),
    dropoff_location_id VARCHAR(16)
  );

describe trips;

select distinct(pickup_location_id) from trips;

Exit
```

ajouter données à instance Cloud SQL

```bash
gsutil cp gs://cloud-training/OCBL013/nyc_tlc_yellow_trips_2018_subset_1.csv trips.csv-1
gsutil cp gs://cloud-training/OCBL013/nyc_tlc_yellow_trips_2018_subset_2.csv trips.csv-2
```

importez les données du fichier CSV dans Cloud SQL à l'aide de mysql

```bash
mysql --host=$MYSQLIP --user=root  --password  --local-infile
```

```sql
use bts;
LOAD DATA LOCAL INFILE 'trips.csv-1' INTO TABLE trips
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 LINES
(vendor_id,pickup_datetime,dropoff_datetime,passenger_count,trip_distance,rate_code,store_and_fwd_flag,payment_type,fare_amount,extra,mta_tax,tip_amount,tolls_amount,imp_surcharge,total_amount,pickup_location_id,dropoff_location_id);

LOAD DATA LOCAL INFILE 'trips.csv-2' INTO TABLE trips
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 LINES
(vendor_id,pickup_datetime,dropoff_datetime,passenger_count,trip_distance,rate_code,store_and_fwd_flag,payment_type,fare_amount,extra,mta_tax,tip_amount,tolls_amount,imp_surcharge,total_amount,pickup_location_id,dropoff_location_id);
```