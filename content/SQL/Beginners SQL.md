---
title: '"Beginners SQL"'
draft: false
tags:
---
## Overview

- **SQL:** Structured Query Langauge
- Database --> Schema --> Tables
- Database Management System (DBMS):
    - PostgreSQL
    - MySQL
    - MongoDB
    - MariaDB
    - SQLServer
- Graphical Interface:
    - PgAdmin

## Basic SELECT

```sql
SELECT *
FROM actor

SELECT first_name,last_name
FROM actor
```

## Sorting with ORDER BY

```sql
SELECT first_name, last_name
FROM actor
ORDER BY first_name DESC
--DEC is descending
--ASC is ascending (Default)
```

## SELECT DISTINCT

```sql
SELECT DISTINCT first_name
FROM actor
ORDER BY first_name
--Removes duplicates for all columns selected
```

## LIMIT

```sql
SELECT first_name
FROM actor
ORDER BY first_name
LIMIT 4
--How many results do you want to see?
```

## COUNT

```sql
SELECT
COUNT (*)
FROM actor
--returns number of rows(excluding NULLS)

SELECT
COUNT (DISTINCT first_name)
FROM actor
--returns the number of distinct names
```

## Filtering with WHERE

```sql
SELECT first_name, last_name
FROM customer
WHERE first_name = 'ADAM'
```

```sql
SELECT *
FROM payment
WHERE amount >= 10.99
ORDER BY amount DESC

-- Operators: =, >, <, >=, <=, <>, !=
-- WHERE is null // is not null
```

## AND/OR Condition

```sql
SELECT *
FROM payment
WHERE (amount = 10.99
OR amount = 9.99)
AND customer_id = 426
```

## BETWEEN

```sql
SELECT payment_id, payment_date
FROM payment
WHERE amount
BETWEEN 1.99 and 6.99

--EXCLUSIONS: NOT BETWEEN 1.99 and 6.99
--DATES: BETWEEN '2020-01-24' AND '2020-01-26'
-- DEFAULTS to Midnight '2020-01-26 0:00'
--To get the full date --> '2020-01-26 23:59'
```

## IN

```sql
SELECT *
FROM customer
WHERE customer_id
IN (123,212,323,243,353,432)

-- NOT IN
```

## LIKE

```sql
--filter by matching against a pattern
SELECT *
FROM actor
WHERE first_name
LIKE 'A%' --All names starting with 'A', case senstive

SELECT *
FROM actor
WHERE first_name
LIKE '_A%' --All names starting with second character is 'A', case senstive

SELECT *
FROM film
WHERE description LIKE '%Drama%'
AND title LIKE '_T%'

/* Wildcards:
_ for any single character
% for any sequence of characters
- 'ILIKE' is not case sensitive
- NOT LIKE
*/
```

## Aliases and Comments

```sql
-- single line comment

/* Multiline comments are very important because I have so much to say and so many things to do that will take up so many lines.*/

-- Aliases
SELECT payment_id AS invoice_no
FROM payment
```

## Aggregate Functions

- Aggregate functions allow us to summarize multiple lines of data
- The most common aggregate functions are:
    - SUM, AVG, MIN, MAX, COUNT

```sql
SELECT
SUM(amount),
COUNT(*),
ROUND(AVG(amount),3)
FROM payment

-- Count is the only function that doesn't need a column name
--ROUND({number}, {num of decimals})
```

## GROUP BY

- Used to group aggregation by specific columns

```sql
SELECT customer_id,
SUM(amount)
FROM payment
WHERE customer_id > 3
GROUP BY customer_id
ORDER BY customer_id
```

### Multiple Columns

```sql
SELECT staff_id, customer_id,
SUM(amount),
COUNT(*)
FROM payment
--comma seperated
GROUP BY staff_id, customer_id
ORDER BY COUNT(*) DESC
```

DATE --> Timestamp to date only

## HAVING

- Used to filter groupings by aggregations
- Only can be used with a GROUP BY clause

```sql
SELECT customer_id,
SUM(amount)
FROM payment
GROUP BY customer_id
HAVING SUM(amount) > 200
```

#### Setting up new database

1. Right click database and select PSQL Tool
2. Paste the following into the command line and run

```bash
\i '/Users/summer/Library/CloudStorage/GoogleDrive-summer.z.hamilton@gmail.com/My Drive/04 - EDUCATION/Udemy/SQL/flight_database/flight_database.sql'
```

3. Select database and refresh