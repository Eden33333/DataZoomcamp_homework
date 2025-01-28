Here is the [link](https://www.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_green.pdf), you can know the descirption of the data.
1. Prepared the files,  docker-compose.yaml to hold the pgsql's setting, a Dockerfile to contain python docker configuration and a ingest_data.py to download the data from web.
2. Open docker-desktop
3. Run `docker-compose up -d`
4. Run
  ```
  docker run -it \
  --network=pg-network \
  taxi_ingest:v001 \
    --user=root \
    --password=root \
    --host=pg-database \
    --port=5432 \
    --db=ny_taxi \
    --tb=green_taxi_trips_Feb \
    --url=${url}
  ```
Note: 
- Set url="" before step 4(without any space)
- You need to change the `network` name by `docker network ls` and choose `week1_default`
- Change tb
- Change db if you want to change database

# Question 3
During the period of October 1st 2019 (inclusive) and November 1st 2019 (exclusive), how many trips, respectively, happened:
1. Up to 1 mile
2. In between 1 (exclusive) and 3 miles (inclusive),
3. In between 3 (exclusive) and 7 miles (inclusive),
4. In between 7 (exclusive) and 10 miles (inclusive),
5. Over 10 miles
## Answer
1. miles<= 1 --104838
2. 1<miles<=3 --199013
3. 3<miles<=7  -- 109,645
4. 7<miles<=10 --27,688
5. miles>=10 --35,202
As the picture shows, I only execute the #5 and got 35,202 so pick the answer.
![Question 3](https://github.com/Eden33333/DataZoomcamp_homework/blob/main/Week1/Image/Question%203.png)



# Question 4
Which was the pick up day with the longest trip distance? Use the pick up time for your calculations.

Tip: For every day, we only care about one single trip with the longest distance.

2019-10-11
2019-10-24
2019-10-26
2019-10-31
## Answer
We need `max()` fucntion to solve this problem. As we only care about 1 single trip so we can just apply `order by xx desc`, without change the timestamp format to "yyyy-mm-dd"
![Question 4](https://github.com/Eden33333/DataZoomcamp_homework/blob/main/Week1/Image/Question4.png)

# Question 5
Which were the top pickup locations with over 13,000 in total_amount (across all trips) for 2019-10-18?

Consider only lpep_pickup_datetime when filtering by date.

East Harlem North, East Harlem South, Morningside Heights
East Harlem North, Morningside Heights
Morningside Heights, Astoria Park, East Harlem South
Bedford, East Harlem North, Astoria Park

## Answer
Tips: you need to change the time format by `'2019-10-19' ::timestamp` to lock the range
More directly, we can transform `lpep_pickup_datetime` by `to_char("lpep_pickup_datetime", 'YYYY-MM-DD')`
As we need "total_amount (across all trips)", we need to apply aggregate function `sum()` to calculate and group by the zones name/zones ID.
As the answers only contain zone name, we need to use  `left join` to reach our goal.
"over 13,000" seems a useless information, as all three contries is up to 13,000 and there is no misleading answer, like "East Harlem North, East Harlem South"

![Q5](https://github.com/Eden33333/DataZoomcamp_homework/blob/main/Week1/Image/Question%205.png)

# Question 6
For the passengers picked up in October 2019 in the zone named "East Harlem North" which was the drop off zone that had the largest tip?

Note: it's tip , not trip

We need the name of the zone, not the ID.

Yorkville West
JFK Airport
East Harlem North
East Harlem South

## Answer
- Pick-up zone is "East Harlem North": We need use `left join` to make pick-up zone and use it as filter condition
- The largest tip in drop off zone: we need to use aggreate function `sum` to calculate the largest tip in drop-off area. Also, use table zones to map the specific name
The answer below shows the largest tip drop-off zone is **Upper East Side North** but we don't have this option, so we choose the relatively largest **East Harlem South**

![Question 6](https://github.com/Eden33333/DataZoomcamp_homework/blob/main/Week1/Image/Question%206.png)

