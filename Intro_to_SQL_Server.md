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
READ:
SELECT statement

INSERT:


