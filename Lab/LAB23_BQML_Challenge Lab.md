# Engineer Data in Google Cloud: Challenge Lab

## Task 1: Clean your training data

```sql
CREATE OR REPLACE TABLE `taxirides.taxi_training_data_642` AS
SELECT 
pickup_datetime,
pickup_longitude AS pickuplon,
pickup_latitude AS pickuplat,
dropoff_longitude AS dropofflon,
dropoff_latitude AS dropofflat,
passenger_count AS passengers,
tolls_amount + fare_amount AS fare_amount_890 # target column that does not include tips
FROM `qwiklabs-gcp-04-3e2b438147e5.taxirides.historical_taxi_rides_raw` 
WHERE trip_distance > 4
AND fare_amount > 2.0
AND passenger_count > 4
  # Ensure that the latitudes and longitudes are reasonable for the use case.
  # eliminate values equal to 0
  AND pickup_longitude <> 0
  AND pickup_latitude <> 0
  AND dropoff_longitude <> 0
  AND dropoff_latitude <> 0
  #   # normal values
  #   longitude: -180° à +180°, 0° méridien de Greenwich
  #   latitude: -90° au sud à 90° au nord, 0° équateur 
  AND pickup_longitude BETWEEN -180 AND 180 
  AND pickup_latitude BETWEEN -90 AND 90
  AND dropoff_longitude BETWEEN -180 AND 180
  AND dropoff_latitude  BETWEEN -90 AND 90
 
LIMIT 1000000 ## sample dataset to 1 Million rows
```

## Task 2: Create a BQML model

### train

```sql
CREATE OR REPLACE MODEL `taxirides.fare_model_289`
TRANSFORM(
  ST_Distance(ST_GeogPoint(pickuplon, pickuplat), ST_GeogPoint(dropofflon, dropofflat)) AS euclidean,
  pickup_datetime,
  passengers,
  fare_amount_890)
OPTIONS(
input_label_cols=['fare_amount_285'],
model_type='linear_reg')
AS
SELECT * FROM `taxirides.taxi_training_data_642`

  -- RMSE = 12.04
```

```sql
CREATE OR REPLACE MODEL `taxirides.fare_model_289`
TRANSFORM(
  ST_Distance(ST_GeogPoint(pickuplon, pickuplat), ST_GeogPoint(dropofflon, dropofflat)) AS euclidean,
  # type dateitime not accepted
  EXTRACT( DAY FROM pickup_datetime) as day,
  EXTRACT( MONTH FROM pickup_datetime) as month,
  EXTRACT( HOUR FROM pickup_datetime) as hour,
  EXTRACT( DAYOFWEEK FROM pickup_datetime) as DAYOFWEEK,
  passengers,
  fare_amount_890
  )
OPTIONS(
input_label_cols=['fare_amount_890'],
model_type='linear_reg')
AS
SELECT * FROM `taxirides.taxi_training_data_642`
  -- RMSE = 12.72
```

```sql
CREATE OR REPLACE MODEL `taxirides.fare_model_289`
TRANSFORM(
  ST_Distance(ST_GeogPoint(pickuplon, pickuplat), ST_GeogPoint(dropofflon, dropofflat)) AS euclidean,
  # type dateitime not accepted
  EXTRACT( DAY FROM pickup_datetime) as day,
  EXTRACT( MONTH FROM pickup_datetime) as month,
  EXTRACT( HOUR FROM pickup_datetime) as hour,
  EXTRACT( DAYOFWEEK FROM pickup_datetime) as DAYOFWEEK,
  passengers,
  fare_amount_890
  )
OPTIONS(
input_label_cols=['fare_amount_890'],
model_type='BOOSTED_TREE_REGRESSOR')
AS
SELECT * FROM `taxirides.taxi_training_data_642`
  -- RMSE = 6.49
```

### Evaluate

```sql
--standardSQL
SELECT
  SQRT(mean_squared_error) AS rmse
FROM
  ML.EVALUATE(MODEL `taxirides.fare_model_289`,
    (SELECT
        *
      FROM 
        `taxirides.taxi_training_data_642` ) )
```

## Task 3: Perform a batch prediction on new data

```sql
CREATE OR REPLACE TABLE `taxirides.2015_fare_amount_predictions`
AS
SELECT * FROM 
ML.PREDICT(
  MODEL `taxirides.fare_model_289`,
  TABLE `taxirides.report_prediction_data`
)
```