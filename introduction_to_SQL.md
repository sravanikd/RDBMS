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
#### WHERE IN
```sql
SELECT name
FROM kids
WHERE age IN (2, 4, 6, 8, 10);
```

##### NULL and IS NULL
In SQL, NULL represents a missing or unknown value. You can check for NULL values using the expression IS NULL. For example, to count the number of missing birth dates in the people table:

```SQL
SELECT COUNT(*)
FROM people
WHERE birthdate IS NULL;
As you can see, IS NULL is useful when combined with WHERE to figure out what data you're missing.
```

Sometimes, you'll want to filter out missing values so you only get results which are not NULL. To do this, you can use the IS NOT NULL operator.

For example, this query gives the names of all people whose birth dates are not missing in the people table.
```SQL
SELECT name
FROM people
WHERE birthdate IS NOT NULL;
```


##### LIKE and NOT LIKE
As you've seen, the WHERE clause can be used to filter text data. However, so far you've only been able to filter by specifying the exact text you're interested in. In the real world, often you'll want to search for a pattern rather than a specific text string.

In SQL, the LIKE operator can be used in a WHERE clause to search for a pattern in a column. To accomplish this, you use something called a wildcard as a placeholder for some other values. There are two wildcards you can use with LIKE:

The % wildcard will match zero, one, or many characters in text. For example, the following query matches companies like 'Data', 'DataC' 'DataCamp', 'DataMind', and so on:
```sql
SELECT name
FROM companies
WHERE name LIKE 'Data%';
```

The _ wildcard will match a single character. For example, the following query matches companies like 'DataCamp', 'DataComp', and so on:
```sql
SELECT name
FROM companies
WHERE name LIKE 'DataC_mp';
```
You can also use the NOT LIKE operator to find records that don't match the pattern you specify.


##### Aggregate functions
Often, you will want to perform some calculation on the data in a database. SQL provides a few functions, called aggregate functions, to help you out with this.

For example,
```sql
SELECT AVG(budget)
FROM films;
gives you the average value from the budget column of the films table. Similarly, the MAX function returns the highest budget:

SELECT MAX(budget)
FROM films;
The SUM function returns the result of adding up the numeric values in a column:

SELECT SUM(budget)
FROM films;
You can probably guess what the MIN function does! Now it's your turn to try out some SQL functions.
```

##### Aggregate functions with WHERE
Aggregate functions can be combined with the WHERE clause to gain further insights from your data.

For example, to get the total budget of movies made in the year 2010 or later:
```sql
SELECT SUM(budget)
FROM films
WHERE release_year >= 2010;
```

Example: Get the average amount grossed by all films whose titles start with the letter 'A'.
```sql
select avg(gross) from films where title like 'A%'
```

Example: Get the amount grossed by the best performing film between 2000 and 2012, inclusive.
```sql
select max(gross) from films where release_year between 2000 and 2012;
```

##### Arithmetic
In addition to using aggregate functions, you can perform basic arithmetic with symbols like +, -, *, and /.

So, for example, this gives a result of 12:
```sql
SELECT (4 * 3);
```
However, the following gives a result of 1:
```sql
SELECT (4 / 3);
```
What's going on here?

SQL assumes that if you divide an integer by an integer, you want to get an integer back. So be careful when dividing!

If you want more precision when dividing, you can add decimal places to your numbers. For example,
```sql
SELECT (4.0 / 3.0) AS result;
```
gives you the result you would expect: 1.333

#####  AS aliasing
```sql
SELECT MAX(budget)
FROM films;
```
gives you a result with one column, named max. But what if you use two functions like this?
```sql
SELECT MAX(budget), MAX(duration)
FROM films;
```
Well, then you'd have two columns named max, which isn't very useful!

To avoid situations like this, SQL allows you to do something called aliasing. Aliasing simply means you assign a temporary name to something. To alias, you use the AS keyword, which you've already seen earlier in this course.

For example, in the above example we could use aliases to make the result clearer:
```sql
SELECT MAX(budget) AS max_budget,
       MAX(duration) AS max_duration
FROM films;
```
