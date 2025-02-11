# Homework

## Question 1

Answer: 128.3 MB

## Question 2

Answer: green_tripdata_2020-04.csv

## Question 3
SELECT COUNT (*) 
FROM public.yellow_tripdata 
WHERE filename ILIKE ('yellow_tripdata_2020_%')

Answer: 24,648,499

## Question 4
SELECT COUNT (*) 
FROM public.yellow_tripdata 
WHERE filename ILIKE ('green_tripdata_2020_%')

Answer: 1,734,051

## Question 5
SELECT COUNT (*)
FROM public.yellow_tripdata
WHERE filename ILIKE ('yellow_tripdata_2021_03%')

Answer: 1,925,152

## Question 6

Answer: Add a timezone property set to America/New_York in the Schedule trigger configuration