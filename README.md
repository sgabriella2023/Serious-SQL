
# Gabi's Serious SQL Notes

![SQL Query Meme for My Notes](https://media.makeameme.org/created/my-sql-query.jpg)

Hi I'm Gabi & these are my SQL notes from Data with Danny's [Serious SQL Course](https://www.datawithdanny.com/)

Additional Resources:
* [PostGresSQL CheatSheet](https://www.postgresqltutorial.com/postgresql-cheat-sheet/)
* [Identity Column](https://www.postgresqltutorial.com/postgresql-identity-column/)



# Select & Sort in SQL

Select is used to state the specific columns we want to pull from a table

Tip: Be careful on comma usage and spelling of columns

## **Select Example**

```
SELECT * 
FROM dvd_rentals.language; 
```

Tip: ctrl + f to highlight all the commas via search

## **LIMIT + Example**

Using LIMIT because we are dealing with big data so we don't know how big the rows can get

I.e. Kt.dbs and her tiktok: [Data set is too big for excel ](https://www.tiktok.com/@kt.dbs/video/7033383736058301742?is_from_webapp=1&sender_device=pc&web_id6912244822627927557)

```
Select *
FROM dvd_rentals.actor
LIMIT 10;
```

## **Sort + Example**

* Use Order By at the end of queries

* Default is Ascending order

* Can do multi-level sorting by specifying multiple columns

* Unless otherwise stated in PostGresSQL, nulls display last

* Use ```NULLS FIRST```

* Different sort orders based on text type data points (See Appendix)

```
SELECT country
FROM dvd_rentals.country
ORDER BY country
LIMIT 5;
```

How do we specify descending order for 
>Order By 

``` 
Select 
    category,
    total_sales
FROM dvd_rentals.sales_by_film_category
ORDER BY total_sales DESC
LIMIT 1; 
```
## **Sort by Multiple Columns**

Sample Code Block
```
DROP TABLE IF EXISTS sample_table;
CREATE TEMP TABLE sample_table AS
WITH raw_data (id, column_a, column_b) AS (
 VALUES
 (1, 0, 'A'),
 (2, 0, 'B'),
 (3, 1, 'C'),
 (4, 1, 'D'),
 (5, 2, 'D'),
 (6, 3, 'D')
)
SELECT * FROM raw_data;
```

Breakdown of code block

> DROP TABLE IF EXISTS sample_table;

What does "Drop table if exists" mean? 

> CREATE TEMP TABLE sample_table AS

Now we are creating a temp table or duplicate of this table??? 

> WITH raw_data (id, column_a, column_b) AS (

What does ID do? ID is indicating the sort order for these 2 columns (with the identity column as ID)

We are assigning values to the columns! 

```
VALUES
    (1, 0, 'A'),
    (2, 0, 'B'),
    (3, 1, 'C'),
    (4, 1, 'D'),
    (5, 2, 'D'),
    (6, 3, 'D')
    )
SELECT * FROM raw_data;
```

Tip: You can specify sort order with DESC for a specific column when sorting 2 columns 

**Example**
```
SELECT * FROM sample_table
ORDER BY column_a DESC, column_b
```

## Sorting Examples

1. What is the name of the category with the highest category_id in the dvd_rentals.category table?

Travel - 16 

```
SELECT
  name,
  category_id
FROM dvd_rentals.category
ORDER BY category_id DESC;
```

2. For the films with the longest length, what is the title of the “R” rated film with the lowest replacement_cost in dvd_rentals.film table?

Home Pity, R, 185 lenght, $15.99

My Code
```
SELECT
  title,
  rating,
  length,
  replacement_cost
FROM dvd_rentals.film
ORDER BY length DESC, replacement_cost;
```

Solution Code
```
SELECT
  title,
  replacement_cost,
  length,
  rating
FROM dvd_rentals.film
ORDER BY length DESC, replacement_cost
LIMIT 10;
```
* Different column order
* Added a limit of 10 which is helpful


3. Who was the manager of the store with the highest total_sales in the dvd_rentals.sales_by_store table?

Jon Stephens
33726.77

My Code (matches solution)
```
SELECT 
  manager,
  total_sales
FROM dvd_rentals.sales_by_store
ORDER BY total_sales DESC;
```

4. What is the postal_code of the city with the 5th highest city_id in the dvd_rentals.address table?

City_ID 596
Postal_Code 31390

My Code 
```
SELECT 
  city_id,
  postal_code
FROM dvd_rentals.address
ORDER BY city_ID DESC
LIMIT 5;
```

Solution Code
```
SELECT
  postal_code,
  city_id
FROM dvd_rentals.address
ORDER BY city_id DESC;
```
* The Column order is different which makes it easier to see the rank of city_ID
* no limit for this one which is interesting

## Additional Notes 

```
WITH test_data (sample_values) AS (
VALUES
(null),
('0123'),
('_123'),
(' 123'),
('(abc'),
('  abc'),
('bca')
)
SELECT * FROM test_data
ORDER BY 1;
```

This block of code demonstrates when sorting text fields.
* general sorting: 0123 comes up first
* Nulls FIRST: null is first
* ORDER BY 1 DESC NULLS FIRST: null then bca