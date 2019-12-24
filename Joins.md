##### Inner Join
Only joins values with common key from all the tables we are joining
Only keeps only the records IN both the tables

```sql
SELECT *
FROM left_table
INNER JOIN right_table
ON left_table.id = right_table.id;
```
```sql
SELECT c1.name AS city, c2.name AS country
FROM cities AS c1
INNER JOIN countries AS c2
ON c1.country_code = c2.code;
```
The ability to combine multiple joins in a single query is a powerful feature of SQL, e.g:
```sql
SELECT *
FROM left_table
INNER JOIN right_table
ON left_table.id = right_table.id
INNER JOIN another_table
ON left_table.id = another_table.id;
```

##### Inner join with USING
while joining, if the id that we are joining on is the same name, we can use USING keyword
USING (id);
When joining tables with a common field name, e.g.

SELECT *
FROM countries
  INNER JOIN economies
    ON countries.code = economies.code
You can use USING as a shortcut:
```sql
SELECT *
FROM countries
  INNER JOIN economies
    USING(code)
```
##### SELF JOINS
where a table joins with itself
Exercise:  use the populations table to perform a self-join to calculate the percentage increase in population from 2010 to 2015 for each country code!

Since you'll be joining the populations table to itself, you can alias populations as p1 and also populations as p2. This is good practice whenever you are aliasing and your tables have the same first letter.
Note that you are required to alias the tables with self-joins.
```sql
SELECT p1.country_code,
       p1.size AS size2010, 
       p2.size AS size2015,
       -- 1. calculate growth_perc0
       ((p2.size- p1.size)/p1.size) * 100.0 AS growth_perc
-- 2. From populations (alias as p1)
FROM populations AS p1
  -- 3. Join to itself (alias as p2)
  INNER JOIN populations AS p2
    -- 4. Match on country code
    ON p1.country_code = p2.country_code
        -- 5. and year (with calculation)
        AND p1.year = p2.year - 5;
        ```
##### Case when and then
Often it's useful to look at a numerical field not as raw data, but instead as being in different categories or groups.

You can use CASE with WHEN, THEN, ELSE, and END to define a new grouping field.

Exercise: Using the countries table, create a new field AS geosize_group that groups the countries into three groups:
If surface_area is greater than 2 million, geosize_group is 'large'.
If surface_area is greater than 350 thousand but not larger than 2 million, geosize_group is 'medium'.
Otherwise, geosize_group is 'small'
```sql
SELECT name, continent, code, surface_area,
    -- 1. First case
    CASE WHEN surface_area > 2000000 THEN 'large'
        -- 2. Second case
        WHEN surface_area > 350000 THEN 'medium'
        -- 3. Else clause + end
        ELSE 'small' END
        -- 4. Alias name
        AS geosize_group
-- 5. From table
FROM countries;
```
Exercise:
Step1:
Using the populations table focused only for the year 2015, create a new field aliased as popsize_group to organize population size into
'large' (> 50 million),
'medium' (> 1 million), and
'small' groups.
Select only the country code, population size, and this new popsize_group as fields.
```sql
SELECT country_code, size,
    -- 1. First case
    CASE WHEN size > 50000000 THEN 'large'
        -- 2. Second case
        WHEN size > 1000000 THEN 'medium'
        -- 3. Else clause + end
        ELSE 'small' END
        -- 4. Alias name (popsize_group)
        AS popsize_group
-- 5. From table
FROM populations
-- 6. Focus on 2015
WHERE year = 2015;
```
Step2:
Use INTO to save the result of the previous query as pop_plus. 
You can see an example of this in the countries_plus code in the assignment text. 
Make sure to include a ; at the end of your WHERE clause!
Then, include another query below your first query to display all the records in pop_plus using SELECT * FROM pop_plus; 
so that you generate results and this will display pop_plus in query result.

SELECT country_code, size,
  CASE WHEN size > 50000000
            THEN 'large'
       WHEN size > 1000000
            THEN 'medium'
       ELSE 'small' END
       AS popsize_group
INTO pop_plus       
FROM populations
WHERE year = 2015;

Step 3:
Keep the first query intact that creates pop_plus using INTO.
Write a query to join countries_plus AS c on the left with pop_plus AS p on the right matching on the country code fields.
Sort the data based on geosize_group, in ascending order so that large appears on top.
Select the name, continent, geosize_group, and popsize_group fields.
```sql
SELECT country_code, size,
  CASE WHEN size > 50000000
            THEN 'large'
       WHEN size > 1000000
            THEN 'medium'
       ELSE 'small' END
       AS popsize_group
INTO pop_plus       
FROM populations
WHERE year = 2015;

-- 5. Select fields
SELECT name,continent,geosize_group,popsize_group 
-- 1. From countries_plus (alias as c)
FROM countries_plus AS c
  -- 2. Join to pop_plus (alias as p)
  INNER JOIN pop_plus AS p
    -- 3. Match on country code
    ON c.code = p.country_code
-- 4. Order the table    
ORDER BY geosize_group;
```

#####  OUTER JOIN
reaching OUT to another table while keeping all records of the original table
3 types: LEFT JOIN,RIGHT JOIN, FULL JOINS


##### FULL JOIN
Includes all records from both the tables

##### SET THEORY
Fields should be of same data type
UNION: does include all records of one copy from all tables
UNION ALL: includes all records including duplicates from all tables
UNION & UNION ALL stack records on top of each other from one table to the next
Example:
```sql
-- Select field
select country_code
  -- From cities
  from cities
	-- Set theory clause
	union 
-- Select field
select code
  -- From currencies
  from currencies
-- Order by country_code
order by country_code;
```
selects country code from cities table and unions them with all the fields from currencies excluding duplicates of code(country code)

INTERSECT: looks for the fields from 2 tables that have same "values" in rows.
Example: which countries also have a city with the same name as their country name?
```sql
-- Select fields
select name
  -- From countries
  from countries
	-- Set theory clause
	intersect
-- Select fields
select name
  -- From cities
  from cities;
  ```
 o/p: Singapore
 
EXCEPT:
only records that appear on the left table but do not appear on the right table are included

Example: Get the names of cities in cities which are not noted as capital cities in countries as a single field result.
```sql
-- Select field
SELECT name
  -- From cities
  FROM cities
	-- Set theory clause
	except
-- Select field
SELECT capital
  -- From countries
  FROM countries
-- Order by result
ORDER BY name;
```

SEMI JOIN, ANTI JOIN:
Use right table to determine which records to keep on the left table
Semi Join: chooses records in the first table where a condition "IS" met in a second table
Anti Join: chooses records in the first table where a condition "IS NOT" met in a second table

