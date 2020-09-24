# Homework 2 -- Analytics with PostGIS

This homework will rely strongly on PostgreSQL and PostGIS documentation. Also refer to recommended readings, past lectures and labs.

**NOTE:** I take logic for solving problems into account when grading. When in doubt, write your thinking for solving the problem even if you aren't able to get a full response.

**NOTE:** If you run out of space in your carto account, you can delete tables you no longer use, or email me (andyepenn@design.upenn.edu) to get more storage space.

## Homework Problems

1. Which bus stops have the largest/smallest population within 800 meters? As a rough estimation, consider any block group that intersects the buffer as being part of the 800 meter buffer. Use 0.008 degrees as an approximation for 800 meters to avoid using geography which will slow the query and potentially cause DB timeouts. See problem 10 if you want to use `geography` without timeouts.

```SQL
-- write your query here
```

2. Using an 800m buffer around each bus stop, intersect with census block groups to find the ratio of the area of spatial intersection of the buffer with the original area of the block group polygon. Multiply this ratio by the total population of the census block groups. Finally, sum up the result to get an estimated population within the radius. _If you get query timeouts, choose about a dozen bus stops in and around Penn's campus. Once you're successful on the query that's limited, write the full query below._

Include the `stop_name`, estimated population, and point geometry. Order by estimated population (largest on top).

```SQL
-- write your query response here
```

**Answer:** Create in the table below with the first five rows of the results, even if just a sample of bus stops.

| stop_name | estimated_pop_800m | geom |
|-----------|--------------------|------|
| a | 10 | Point(...) |


3. Using the OSM Buildings dataset, pair each building with its closest bus stop. The final result should give the building name, bus stop name, and distance apart in meters. Order by distance (largest on top).

```SQL
-- write your query response here
```

**Answer:** Fill in the table below for the five buildings furthest from a bus stop.

| stop_name | osm_id | distance_apart |
|-----------|--------|----------------|
| a | 1012321 | 900 |


4. Using the `shapes.txt` file from GTFS bus feed, find the two routes with the longest trips (in meters). In the final query, give the `trip_headsign` that corresponds to the `shape_id` of this route and the length of the trip. You can use the [Carto batch SQL API tool](https://cartodb.github.io/customer_success/batch/) to do the geometry updates (`UPDATE table SET the_geom = ...`) to avoid DB timeouts.

```SQL
-- write your query here
```

4. Rate neighborhoods by their bus stop accessibility for wheelchairs. Use the [neighborhood dataset from the open data portal](https://www.opendataphilly.org/dataset/philadelphia-neighborhoods) along with an appropriate dataset from the Septa GTFS bus feed. Use the [GTFS documentation](https://gtfs.org/reference/static/) for help. Use some creativity in the metric you devise in rating neighborhoods.

```SQL
-- write your query here
```

**Answer:** Fill in the table below for the **top** two neighborhoods.

| neighborhood_name | accessibility_metric | num_bus_stops_accessible | num_bus_stops_inaccessible |
|-------------------|----------------------|--------------------------|----------------------------|
| a | 87 | 37 | 4 |

Fill in the table below for the **bottom** two neighborhoods.

| neighborhood_name | accessibility_metric | num_bus_stops_accessible | num_bus_stops_inaccessible |
|-------------------|----------------------|--------------------------|----------------------------|
| a | 87 | 37 | 4 |


5. With a query, find out how many census block groups Penn's main campus fully contains. Discuss which dataset you chose for defining Penn's campus.

```SQL
-- write your query here
```

**Answer:**

6. With a query involving OSM buildings and census block groups, find the geoid of Meyerson Hall. ST_MakePoint() and functions like that are not allowed.

```SQL
-- write your query here
```

**Answer:**

7. Expand the `device_home_areas` stringified JSON entry of the Safegraph Neighborhood Patterns row corresponding to Meyerson Hall's geoid into a table using an appropriate PostgreSQL JSON function. Table aliases can alias column names (e.g., "SELECT * FROM (SELECT 1 as a, 2 as b) as t(c, d)" renames the columns a and b to c and d.) Order by origin geoid and the geoid of the home devices.

Hint: You can cast a well-formed json string into a json object using `::json` type casting.

```SQL
-- write your query here
```
Create a markdown table to store the first five entries of the query result.

**Answer:**

8. Using the result from \#7, write a query that gives the distance (in meters) from Meyerson Hall's block group to the census block groups of all the `device_home_areas`, the number of devices, and a linestring geometry connecting the centroids of each device home to Meyerson Hall's block group.

```sql
-- write your query here
```


9. You're tasked with giving more contextual information to rail stops to fill the `stop_desc` field in a GTFS feed. Using OSM building data, PostGIS functions (e.g., `ST_Distance`, `ST_Azimuth`), and postgres string functions, build a description (alias as `stop_desc`) paired with `stop_id`, `stop_name`, `stop_lon`, and `stop_lat`. Feel free to supplement with other datasets (must provide link to data used so it's reproducible), and other methods of describing the relationships. Postgres' `CASE` statements may be helpful for some operations.

**Tip when experimenting:** Use subqueries to limit your query to just a few rows to keep query times faster. Once your query is giving you answers you want, scale it up. E.g., instead of `FROM tablename`, use `FROM (SELECT * FROM tablename limit 10) as t`.

Final query results should be in this form (`stop_desc` is only an example, feel free to be creative, silly, descriptive, etc.):

```
stop_id | stop_name |           stop_desc          | stop_lon | stop_lat
1       | SQL Road  | 37 meters NE of ABC Building | 0        | 0
```

## Bonus Question

10. Create an index on the geography for the queries involved in questions 1 and 2. Time the queries before and after having the geography index using the tool we used in class (<https://cartodb.github.io/customer_success/batch/>). Write the expression you used to create the geography index, and provide screenshots highlighting the performance of the queries.

