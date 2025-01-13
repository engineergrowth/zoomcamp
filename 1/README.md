## 1
rm

## 2
pip 24.3.1 from /usr/local/lib/python3.12/site-packages/pip (python 3.12)

## 3
SELECT COUNT(*)
FROM green_tripdata
WHERE DATE(lpep_pickup_datetime) = '2019-10-18'
  AND DATE(lpep_dropoff_datetime) = '2019-10-18';

"17417"

## 4

SELECT DATE(lpep_pickup_datetime) AS pickup_date,
       MAX(trip_distance) AS max_trip_distance
FROM green_tripdata
GROUP BY pickup_date
ORDER BY max_trip_distance DESC
LIMIT 1;

"2019-10-31"

## 5

SELECT t1.PULocationID, t2.Zone, SUM(t1.total_amount) AS total_amount
FROM green_tripdata t1
JOIN taxi_zone_lookup t2 ON t1.PULocationID = t2.LocationID
WHERE DATE(t1.lpep_pickup_datetime) = '2019-10-18'
GROUP BY t1.PULocationID, t2.Zone
HAVING SUM(t1.total_amount) > 13000
ORDER BY total_amount DESC
LIMIT 3;

"East Harlem North"
"East Harlem South"
"Morningside Heights"

## 6

SELECT t2.Zone, MAX(t1.tip_amount) AS max_tip
FROM green_tripdata t1
JOIN taxi_zone_lookup t2 ON t1.DOLocationID = t2.LocationID
WHERE DATE(t1.lpep_pickup_datetime) BETWEEN '2019-10-01' AND '2019-10-31'
  AND t1.PULocationID = (SELECT LocationID FROM taxi_zone_lookup WHERE Zone = 'East Harlem North')
GROUP BY t2.Zone
ORDER BY max_tip DESC
LIMIT 1;

"JFK Airport"

## 7 

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.