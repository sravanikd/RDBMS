## SQL

 SQL: Language for interacting with data stored in relational database

 Relational DB: Collection of tables. rows/records | columns/fields

```sql
SELECT * FROM people limit 10;
```
##### select distinct
```sql
SELECT DISTINCT language FROM films;
```
##### Count
```sql
SELECT COUNT(DISTINCT birthdate) FROM reviews;
```
##### Filtering results
In SQL, the WHERE keyword allows you to filter based on both text and numeric values in a table. There are a few different comparison operators you can use:

= equal
<> not equal
< less than
> greater than
<= less than or equal to
>= greater than or equal to
For example, you can filter text records such as title. The following code returns all films with the title 'Metropolis':
```sql
SELECT title
FROM films
WHERE title = 'Metropolis';
```
##### WHERE AND
```sql
SELECT title
FROM films
WHERE release_year > 1994 AND release_year < 2000;
```
```sql
select * from films where language = 'Spanish' and release_year >2000;
```
below code is invalid, to select a range, you can't select without specifying column name to filter out results between a range
Invalid:
select * from films where release_year > 2000 and < 2010
and  language = 'Spanish';

##### WHERE & OR
What if you want to select rows based on multiple conditions where some but not all of the conditions need to be met? For this, SQL has the OR operator.

For example, the following returns all films released in either 1994 or 2000:
```sql
SELECT title
FROM films
WHERE release_year = 1994
OR release_year = 2000;
```
Note that you need to specify the column for every OR condition, so the following is invalid:

SELECT title
FROM films
WHERE release_year = 1994 OR 2000;

##### WHERE, AND & OR
When combining AND and OR, be sure to enclose the individual clauses in parentheses, like so:
```sql
SELECT title
FROM films
WHERE (release_year = 1994 OR release_year = 1995)
AND (certification = 'PG' OR certification = 'R');
```

##### BETWEEN
the BETWEEN keyword provides a useful shorthand for filtering values within a specified range. This query is equivalent to the one above:
```sql
SELECT title
FROM films
WHERE release_year
BETWEEN 1994 AND 2000;
```
It's important to remember that BETWEEN is inclusive, meaning the beginning and end values are included in the results!

example query including between, where, and, or:
```sql
select title, release_year from films
where release_year between 1990 and 2000 
and budget > 100000000
and (language = 'Spanish' or language = 'French');
```
