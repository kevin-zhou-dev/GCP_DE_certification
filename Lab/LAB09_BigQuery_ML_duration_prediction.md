# Lab : BQML Regression model with bike trips

```sql
SELECT
  start_station_name,
  AVG(duration) AS duration
FROM
  `bigquery-public-data`.london_bicycles.cycle_hire
GROUP BY
  start_station_name
```

```sql
SELECT
  EXTRACT(dayofweek
  FROM
    start_date) AS dayofweek,
  AVG(duration) AS duration
FROM
  `bigquery-public-data`.london_bicycles.cycle_hire
GROUP BY
  dayofweek
```

```sql
SELECT
  bikes_count,
  AVG(duration) AS duration
FROM
  `bigquery-public-data`.london_bicycles.cycle_hire
JOIN
  `bigquery-public-data`.london_bicycles.cycle_stations
ON
  cycle_hire.start_station_name = cycle_stations.name
GROUP BY
  bikes_count
```

training dataset creation

```bash
bq --location=eu mk bike_model
```

simple linear regression model with 3 features : start station name, dayofweek, hourofday

```sql
CREATE OR REPLACE MODEL
  bike_model.model
OPTIONS
  (input_label_cols=['duration'],
    model_type='linear_reg') AS
SELECT
  duration,
  start_station_name,
  CAST(EXTRACT(dayofweek
    FROM
      start_date) AS STRING) AS dayofweek,
  CAST(EXTRACT(hour
    FROM
      start_date) AS STRING) AS hourofday
FROM
  `bigquery-public-data`.london_bicycles.cycle_hire
```

model metrics

```sql
SELECT * FROM ML.EVALUATE(MODEL `bike_model.model`)
```

model with combined dayofweeks

```sql
CREATE OR REPLACE MODEL
  bike_model.model_weekday
OPTIONS
  (input_label_cols=['duration'],
    model_type='linear_reg') AS
SELECT
  duration,
  start_station_name,
IF
  (EXTRACT(dayofweek
    FROM
      start_date) BETWEEN 2 AND 6,
    'weekday',
    'weekend') AS dayofweek,
  CAST(EXTRACT(hour
    FROM
      start_date) AS STRING) AS hourofday
FROM
  `bigquery-public-data`.london_bicycles.cycle_hire
```

```sql
SELECT * FROM ML.EVALUATE(MODEL `bike_model.model_weekday`)
```

model with segmented hourofday

```sql
CREATE OR REPLACE MODEL
  bike_model.model_bucketized
OPTIONS
  (input_label_cols=['duration'],
    model_type='linear_reg') AS
SELECT
  duration,
  start_station_name,
IF
  (EXTRACT(dayofweek
    FROM
      start_date) BETWEEN 2 AND 6,
    'weekday',
    'weekend') AS dayofweek,
  ML.BUCKETIZE(EXTRACT(hour
    FROM
      start_date),
    [5, 10, 17]) AS hourofday
FROM
  `bigquery-public-data`.london_bicycles.cycle_hire
```

```sql
SELECT * FROM ML.EVALUATE(MODEL `bike_model.model_bucketized`)
```

Predictions with the best model

"transform" clause that allows to memorize data operations before training - similar to pipelines

```sql
CREATE OR REPLACE MODEL
  bike_model.model_bucketized TRANSFORM(* EXCEPT(start_date),
  IF
    (EXTRACT(dayofweek
      FROM
        start_date) BETWEEN 2 AND 6,
      'weekday',
      'weekend') AS dayofweek,
    ML.BUCKETIZE(EXTRACT(HOUR
      FROM
        start_date),
      [5, 10, 17]) AS hourofday )
OPTIONS
  (input_label_cols=['duration'],
    model_type='linear_reg') AS
SELECT
  duration,
  start_station_name,
  start_date
FROM
  `bigquery-public-data`.london_bicycles.cycle_hire
```

```sql
SELECT
  *
FROM
  ML.PREDICT(MODEL bike_model.model_bucketized,
    (
    SELECT
      'Park Lane , Hyde Park' AS start_station_name,
      CURRENT_TIMESTAMP() AS start_date) )
```

prediction on a dataset of 100  

```sql
SELECT
  *
FROM
  ML.PREDICT(MODEL bike_model.model_bucketized,
    (
    SELECT
      start_station_name,
      start_date
    FROM
      `bigquery-public-data`.london_bicycles.cycle_hire
    LIMIT
      100) )
```

```sql
SELECT * FROM ML.WEIGHTS(MODEL bike_model.model_bucketized)
```
