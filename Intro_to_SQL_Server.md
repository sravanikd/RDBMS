##### SELECT
```sql
SELECT * FROM table_name;
```

- Return 5 rows:
```sql
SELECT TOP(5) artist
FROM artists;
```
- Return top 5% of rows:
```sql
SELECT TOP(5) PERCENT artist
FROM artists;
```

##### CREATE,UPDATE TABLES| CRUD| INSERT
CREATE:
Databases,tables,views
Users, permissions, security groups
```sql
CREATE TABLE test_table(
  test_date DATE, 
  test_name VARCHAR(20), 
  test_int INT
)
```
```sql
-- Select the album
SELECT 
  title 
FROM 
  album 
WHERE 
  album_id = 213;
-- UPDATE the title of the album
UPDATE 
  album 
SET 
  title = 'Pure Cult: The Best Of The Cult' 
WHERE 
  album_id = 213;
-- Run the query again
SELECT 
  title 
FROM 
  album ;
```

##### Variables
- Rather than changing the query to have new results each time, create a variable and change the values in it
```SELECT * FROM artist
WHERE name= @my_artist;
```
-DECLARE:
```sql
DECLARE @
---Integer variable
DECLARE @test_int INT
---VARCHAR variable
DECLARE @my_artist VARCHAR(100)
```

-SET:
Integer variable:
```sql
DECLARE @test_int INT

SET @test_int=5
```
