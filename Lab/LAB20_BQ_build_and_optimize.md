# Build and Optimize Data Warehouses with BigQuery: Challenge Lab

https://partner.cloudskillsboost.google/focuses/14827?locale=en&parent=catalog

## Task 1

CREATE TABLE covid_635.oxford_policy_tracker_487

PARTITION BY date
OPTIONS (partition_expiration_days = 720)
AS 
SELECT * from `bigquery-public-data.covid19_govt_response.oxford_policy_tracker`
where country_name NOT IN ("Brazil","United Kingdom","Canada","United States")


## Task 2 


ALTER TABLE  covid_635.oxford_policy_tracker_487
ADD COLUMN population INTEGER,
ADD COLUMN country_area FLOAT64,
ADD COLUMN mobility STRUCT<
  avg_retail FLOAT64,
  avg_grocery FLOAT64,
  avg_parks FLOAT64,
  avg_transit FLOAT64,
  avg_workplace FLOAT64,
  avg_residential FLOAT64>,
  
## Task 3


UPDATE
    covid_635.oxford_policy_tracker_487 t0
SET
    t0.population = t2.pop_data_2019
FROM
    (SELECT DISTINCT country_territory_code, pop_data_2019 FROM `bigquery-public-data.covid19_ecdc.covid_19_geographic_distribution_worldwide`) AS t2
WHERE t0.alpha_3_code = t2.country_territory_code;

## Task 4 

UPDATE
   covid_635.oxford_policy_tracker_487 t0
SET
    t0.country_area = t2.country_area
FROM
    (SELECT DISTINCT country_name, country_area FROM `bigquery-public-data.census_bureau_international.country_names_area`) AS t2
WHERE t0.country_name = t2.country_name;


## Task 5

UPDATE
   covid_635.oxford_policy_tracker_487 t0
SET
    t0.mobility.avg_retail = t2.avg_retail,
    t0.mobility.avg_grocery = t2.avg_grocery,
    t0.mobility.avg_parks = t2.avg_parks,
    t0.mobility.avg_transit = t2.avg_transit,
    t0.mobility.avg_workplace = t2.avg_workplace,
    t0.mobility.avg_residential = t2.avg_residential

FROM
    ( SELECT country_region, date,
      AVG(retail_and_recreation_percent_change_from_baseline) as avg_retail,
      AVG(grocery_and_pharmacy_percent_change_from_baseline) as avg_grocery,
      AVG(parks_percent_change_from_baseline) as avg_parks,
      AVG(transit_stations_percent_change_from_baseline) as avg_transit,
      AVG( workplaces_percent_change_from_baseline ) as avg_workplace,
      AVG( residential_percent_change_from_baseline)  as avg_residential
      FROM `bigquery-public-data.covid19_google_mobility.mobility_report`
      GROUP BY country_region, date) AS t2
WHERE t0.country_name = t2.country_region AND t0.date = t2.date

## Task 6 

SELECT
  *
FROM (
  SELECT
    DISTINCT country_name,
  FROM
    covid_635.oxford_policy_tracker_487 t0
  WHERE
    country_area IS NULL
  UNION ALL
  SELECT
    DISTINCT country_name,
  FROM
    covid_635.oxford_policy_tracker_487 t0
  WHERE
    population IS NULL )
ORDER BY
  country_name


