# SQL Cheat Sheet

## Basic Queries
```sql
SELECT column1, column2  -- Select specific columns
FROM table_name
WHERE condition;

SELECT DISTINCT column   -- Select unique values
FROM table_name;

SELECT * FROM table_name  -- Select all columns
LIMIT 10;                 -- Limit results
```

## WHERE Conditions
```sql
WHERE column_name = 'value'
WHERE column_name IN (1, 2, 3)
WHERE column_name BETWEEN 1 AND 10
WHERE column_name LIKE 'pattern%'
WHERE column_name IS NULL
WHERE price > 100 AND category = 'books'
WHERE price > 100 OR category = 'books'
```

## JOINS
```sql
-- Inner Join
SELECT * FROM table1
INNER JOIN table2
ON table1.id = table2.id;

-- Left Join
SELECT * FROM table1
LEFT JOIN table2
ON table1.id = table2.id;

-- Right Join
SELECT * FROM table1
RIGHT JOIN table2
ON table1.id = table2.id;

-- Full Outer Join
SELECT * FROM table1
FULL OUTER JOIN table2
ON table1.id = table2.id;
```

## Aggregate Functions
```sql
SELECT 
    COUNT(*),
    SUM(column_name),
    AVG(column_name),
    MAX(column_name),
    MIN(column_name)
FROM table_name;
```

## GROUP BY & HAVING
```sql
SELECT category, COUNT(*) as count
FROM products
GROUP BY category
HAVING count > 5;
```

## ORDER BY
```sql
SELECT *
FROM table_name
ORDER BY column1 ASC, column2 DESC;
```

## INSERT
```sql
INSERT INTO table_name (column1, column2)
VALUES (value1, value2);

INSERT INTO table_name
SELECT * FROM another_table;
```

## UPDATE
```sql
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;
```

## DELETE
```sql
DELETE FROM table_name
WHERE condition;

TRUNCATE TABLE table_name;  -- Delete all rows
```

## Table Operations
```sql
CREATE TABLE table_name (
    column1 datatype CONSTRAINT,
    column2 datatype CONSTRAINT
);

ALTER TABLE table_name
ADD column_name datatype;

DROP TABLE table_name;
```

## Window Functions
```sql
SELECT 
    name,
    salary,
    RANK() OVER (ORDER BY salary DESC) as rank,
    LAG(salary) OVER (ORDER BY hire_date) as prev_salary,
    LEAD(salary) OVER (ORDER BY hire_date) as next_salary
FROM employees;
```

## Common Table Expressions (CTE)
```sql
WITH cte_name AS (
    SELECT column1, column2
    FROM table_name
    WHERE condition
)
SELECT * FROM cte_name;
```

## UNION Operations
```sql
SELECT column FROM table1
UNION
SELECT column FROM table2;

SELECT column FROM table1
UNION ALL
SELECT column FROM table2;
```

## Subqueries
```sql
SELECT *
FROM table1
WHERE column1 IN (
    SELECT column1
    FROM table2
    WHERE condition
);
