# Lab : BQML Movie recommendation with matrix factorization

create dataset and load data

```bash
bq --location=EU mk --dataset movies

bq load --source_format=CSV \
--location=EU \
--autodetect movies.movielens_ratings \
gs://dataeng-movielens/ratings.csv

bq load --source_format=CSV \
--location=EU   \
--autodetect movies.movielens_movies_raw \
gs://dataeng-movielens/movies.csv
```

data exploration

```sql
SELECT
  COUNT(DISTINCT userId) numUsers,
  COUNT(DISTINCT movieId) numMovies,
  COUNT(*) totalRatings
FROM
  movies.movielens_ratings
```

```sql
SELECT
  *
FROM
  movies.movielens_movies_raw
WHERE
  movieId < 5
```

```sql
CREATE OR REPLACE TABLE
  movies.movielens_movies AS
SELECT
  * REPLACE(SPLIT(genres, "|") AS genres)
FROM
  movies.movielens_movies_raw
```

model training (may take up to 40 mins on reserved slots)

```sql
 CREATE OR REPLACE MODEL movies.movie_recommender

OPTIONS (model_type='matrix_factorization', user_col='userId', item_col='movieId', rating_col='rating', l2_reg=0.2, num_factors=16) AS

SELECT userId, movieId, rating

FROM movies.movielens_ratings
```

instead we load a pretrained model

```sql
SELECT * FROM ML.EVALUATE(MODEL `cloud-training-prod-bucket.movies.movie_recommender`)
```

recommendation of movies for user USERID=903
```sql
SELECT
  *
FROM
  ML.PREDICT(MODEL `cloud-training-prod-bucket.movies.movie_recommender`,
    (
    SELECT
      movieId,
      title,
      903 AS userId
    FROM
      `movies.movielens_movies`,
      UNNEST(genres) g
    WHERE
      g = 'Comedy' ))
ORDER BY
  predicted_rating DESC
LIMIT
  5  
```

Delete film already seen by users

```sql
SELECT
  *
FROM
  ML.PREDICT(MODEL `cloud-training-prod-bucket.movies.movie_recommender`,
    (
    WITH
      seen AS (
      SELECT
        ARRAY_AGG(movieId) AS movies
      FROM
        movies.movielens_ratings
      WHERE
        userId = 903 )
    SELECT
      movieId,
      title,
      903 AS userId
    FROM
      movies.movielens_movies,
      UNNEST(genres) g,
      seen
    WHERE
      g = 'Comedy'
      AND movieId NOT IN UNNEST(seen.movies) ))
ORDER BY
  predicted_rating DESC
LIMIT
  5
```

Users targeting ex : we want have more reviews for movieid=96481
we pass all users through the algo and retrieve the highest predicted ratings.
Those users will be targeted for providing additionnal reviews on the film

```sql
SELECT
  *
FROM
  ML.PREDICT(MODEL `cloud-training-prod-bucket.movies.movie_recommender`,
    (
    WITH
      allUsers AS (
      SELECT
        DISTINCT userId
      FROM
        movies.movielens_ratings )
    SELECT
      96481 AS movieId,
      (
      SELECT
        title
      FROM
        movies.movielens_movies
      WHERE
        movieId=96481) title,
      userId
    FROM
      allUsers ))
ORDER BY
  predicted_rating DESC
LIMIT
  100
```

batch predictions for every combinations of users and movies

```sql
SELECT
  *
FROM
  ML.RECOMMEND(MODEL `cloud-training-prod-bucket.movies.movie_recommender`)
LIMIT 
  100000 -- or else there will be too much rows
```
