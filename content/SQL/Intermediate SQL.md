---
title: Intermediate SQL
draft: false
tags:
  - sql
---
# Intermediate
## String Functions
### UPPER, LOWER, LENGTH
```sql
SELECT
UPPER(email) AS email_upper,
LOWER(email) AS email_lower,
LENGTH(email)
FROM customer
WHERE LENGTH(email) < 30
-- UPPER changes to all upper case
-- LOWER changes to all upper case
-- LENGTH shows the number of characters in a string
```

### LEFT & RIGHT
```sql
-- Extract characters from column
SELECT LEFT(first_name, 2), first_name
FROM customer
-- Result: MA, MARY

SELECT RIGHT(first_name, 3), first_name
FROM customer
-- Result: SAN, SUSAN

SELECT RIGHT(LEFT(first_name, 2),1), first_name
FROM customer
-- Result: A, MARY [MA] --> [A]
```

### Concatenation ( || )
```sql
SELECT LEFT(first_name,1) || '.' || LEFT(last_name,1) || '.'
FROM customer
-- Result: Mary Jones --> M.J.
```

### POSITION 
```sql
SELECT
LEFT(email, POSITION('@' IN email)-1), email
FROM customer

-- Result: JANEDOE <-- JANE.DOE@email.com (Position = 9)

SELECT
LEFT(email, POSITION(last_name IN email)-2), email
FROM customer

-- Result: JANE <-- JANE.DOE@email.com (Position = 6)
```

### SUBSTRING
```sql
-- SUBSTRING(string from start [for length])

SELECT email,
SUBSTRING(email from POSITION('.' in email)+1 for LENGTH(last_name))
FROM customer

-- Result: JANE.DOE@email.com --> DOE (Position = 5, Length = 3)
```

### EXTRACT
- Used to EXTRACT parts of timestamp/date
- EXTRACT(field from date/time/interval)

```sql
SELECT
EXTRACT(month from rental_date)
COUNT(*)
FROM rental
GROUP BY EXTRACT(month from rental_date)
ORDER BY COUNT(*) DESC
```

### TO_CHAR
- Used to get custom formats timestamp/date/numbers
- TO_CHAR(date/time/interval, format)
- https://www.postgresql.org/docs/current/functions-formatting.html

```sql
SELECT *,
EXTRACT(month from payment_date),
TO_CHAR(payment_date, 'MM-YYYY')
FROM payment
```

### Intervals and Dates

```sql
--Date
SELECT CURRENT_DATE

--Timestamp
SELECT CURRENT_TIMESTAMP

--Interval
SELECT CURRENT_TIMESTAMP - rental_date
```

## Mathematical Operations
- ` + - * / % `
- Functions:
	- `abs(x)`
	- `round(x,d)`
	- `ceiling(x)`
	- `floor(x)`

```sql
SELECT film_id,
rental_rate as old_rental_rate,
--Increase by 10%
ROUND(()rental_rate*1.1),2) as new_rental_rate
--round to .99
(CEILING(rental_rate*1.1)-0.01) as new_rental_rate
FROM film
```

## CASE WHEN
- Like and If/Then statement
- Goes through a set of conditions, return a value if condition is met

```sql
CASE
WHEN condition1 THEN result1
WHEN condition2 THEN result2
WHEN conditionN THEN resultN
ELSE result
END
```

```sql
SELECT amount,
CASE
	WHEN amount < 2 THEN 'low amount'
	WHEN amount < 5 THEN 'medium amound'
	ELSE 'high amount'
END
FROM rental
```

```sql
SELECT
SUM(
CASE
	WHEN rating IN ('PG', 'G') THEN 1
	ELSE 0
END) as count_ratings
FROM film
```

## COALESCE
- Returns first value of a list of values which is not null
	- replace with value from another column (same data type)
	- replace with hard coded value (same data type)

```sql
SELECT
COALESCE(actual_arrival-scheduled_arrival, '00:00:00')
FROM flight
-- In result, all null values will be replaced with "00:00:00"
```

## CAST
- Changes the data type of a value
- `CAST(column AS data type)`

```sql
SELECT
COALESCE(
	CAST(actual_arrival-scheduled_arrival AS VARCHAR)
	, 'Not Arrived'
)
FROM flight
```

## REPLACE
- Replaces text from a string in a column with different text
- `REPLACE(column, old_text, new_text)`

```sql
SELECT
CAST(REPLACE(passenger_id, ' ', '') AS BIGINT)
FROM tickets
```