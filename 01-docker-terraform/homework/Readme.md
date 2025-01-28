# Homework

## Question 1
docker run -it --entrypoint=bash python:3.12.8

### Answer:
24.3.1

## Question 2
hostname is the name of the database.
The constainer's internal port is 5432 and that's the one we should use inside the docker compose network. Port 5433 will be used to forward the traffics to the container's internal port. 

### Answer:
db:5432

## Prepare Postgres

python ingest_homework_data.py \
  --user=root \
  --password=root \
  --host=localhost \
  --port=5432 \
  --db=ny_taxi \
  --table_name=green_tripdata_2019-10 \
  --url="https://github.com/DataTalksClub/nyc-tlc-data/releases/download/green/green_tripdata_2019-10.csv.gz"

python ingest_homework_data.py \
  --user=root \
  --password=root \
  --host=localhost \
  --port=5432 \
  --db=ny_taxi \
  --table_name=taxi_zone_lookup \
  --url="https://github.com/DataTalksClub/nyc-tlc-data/releases/download/misc/taxi_zone_lookup.csv"

  ## Question 3
  SELECT
    CASE
        WHEN trip_distance <= 1 THEN 'Up to 1 mile'
        WHEN trip_distance > 1 AND trip_distance <= 3 THEN 'In between 1 (exclusive) and 3 miles (inclusive)'
        WHEN trip_distance > 3 AND trip_distance <= 7 THEN 'In between 3 (exclusive) and 7 miles (inclusive)'
        WHEN trip_distance > 7 AND trip_distance <= 10 THEN 'In between 7 (exclusive) and 10 miles (inclusive)'
        ELSE 'Over 10 miles'
    END AS distance_range,
    COUNT(*) AS trip_count
FROM "green_tripdata_2019-10"
WHERE lpep_pickup_datetime >= '2019-10-01'
AND lpep_dropoff_datetime < '2019-11-01'
GROUP BY distance_range
ORDER BY trip_count DESC;

## Answer:
104,802; 198,924; 109,603; 27,678; 35,189

## Question 4
SELECT DATE(lpep_pickup_datetime) as day
FROM public.green_trip_data
WHERE trip_distance = (
    SELECT MAX(trip_distance)
    FROM public.green_trip_data)

Answer: 2019-10-31

## Question 5
SELECT t."Zone" as zone, SUM(total_amount) as sum
FROM public."green_tripdata_2019-10" as g
LEFT JOIN public.taxi_zone_lookup as t
ON g."PULocationID" = t."LocationID"
WHERE g.lpep_pickup_datetime::date = '2019-10-18'
GROUP BY t."Zone"
HAVING SUM(g.total_amount) >= 13000
ORDER BY sum DESC;

Answer: East Harlem North, East Harlem South, Morningside Heights

## Question 6
SELECT MAX(tip_amount), t2."Zone"
FROM public."green_tripdata_2019-10" as g
LEFT JOIN public.taxi_zone_lookup as t1
ON g."PULocationID" = t1."LocationID"
LEFT JOIN public.taxi_zone_lookup as t2
ON g."DOLocationID" = t2."LocationID"
WHERE t1."Zone" = 'East Harlem North'
AND g.lpep_pickup_datetime::date 
BETWEEN '2019-10-01' AND '2019-10-31'
GROUP BY t2."Zone"
ORDER BY MAX(tip_amount) DESC;

Answer: JFK Airport

## Question 7

Answer: terraform init, terraform apply -auto-approve, terraform destroy