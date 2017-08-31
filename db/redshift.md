# Redshift

## Metadata

Return table metadata
```sql
SELECT * FROM pg_table_def
WHERE tablename = {tablename}
```

Show query that generated view
```sql
SELECT definition FROM pg_views WHERE viewname = 'viewname'

SELECT pg_get_viewdef('viewname', true)
```

### VIEW

Create view
```sql
CREATE OR REPLACE VIEW {viewname} AS
SELECT ...
```

## Conditional Expression

### CASE

```sql
-- Simple CASE statement used to match conditions
CASE expression
WHEN value THEN 'result'
  [WHEN...]
  [ELSE 'result']
END

-- Searched CASE statement used to evaluate each condition
CASE
WHEN boolean condition THEN 'result'
  [WHEN ...]
  [ELSE 'result']
END
```

### GREATEST and LEAST

`NULL` values in the list are ignored. If all of the expressions evaluate to `NULL`, the result is `NULL`.
```sql
SELECT LEAST(col1, col2, col3)
```

### COALESCE / NVL

Returns the value of the first expression in the list that is not `NULL`. If all expressions are `NULL`, the result is `NULL`. When a non-`NULL` value is found, the remaining expressions in the list are not evaluated.
```sql
SELECT COALESCE(col1, col2, col3)

SELECT NVL(SUM(sales), 0.0) as sumresult
```

## Date and Time Functions

### DATE_CMP

`DATE_CMP` compares two dates. The returns 0 if the dates are identical, 1 if date1 is greater, and -1 if date2 is greater.
```sql
DATE_CMP(date1, date2)
```

### DATEDIFF

`DATEDIFF` returns the difference between the date parts of two date or time expressions.
```sql
SELECT DATEDIFF(month, '2009-01-01', '2009-12-31') AS nummonths
```

## Data Type Formatting Functions

List of [data types](http://docs.aws.amazon.com/redshift/latest/dg/c_Supported_data_types.html).

### CAST / CONVERT

You can use two equivalent syntax forms to cast expressions from one data type to another. You can also use the `CONVERT` function to convert values from one data type to another:

```sql
CAST(expression AS type)
expression::type

CONVERT(type, expression)
```

### TO_DATE

`TO_DATE` converts a date represented in a character string to a `DATE` data type. See [Datetime Format Strings](http://docs.aws.amazon.com/redshift/latest/dg/r_FORMAT_strings.html).
```sql
TO_DATE('2000-01-01', 'YYYY-MM-DD')
```

## Window Functions

### ROW_NUMBER

Get the most common value by id.
```sql
SELECT
  *
FROM (
    SELECT
        id,
        value,
        ROW_NUMBER() OVER (PARTITION BY id ORDER BY cnt DESC) AS row_number
    FROM (
        SELECT
            id,
            value,
            COUNT(*) AS cnt
        FROM
            table
        GROUP BY
            id,
            value
        )
    )
WHERE row_number = 1
```
