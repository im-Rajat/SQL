# Lesson 13: More Basic Queries

This lesson covers fundamental SQL commands for retrieving, filtering, and sorting data from the company database.

## 1. Basic Selects & Formatting

```sql
-- Find all employees
SELECT *
FROM employee;

-- Find all clients
-- Note: Table name matches the singular 'client' created in Lesson 12
SELECT *
FROM client;

-- Find the first and last names of all employees
SELECT first_name, last_name
FROM employee;

-- Find the forename and surnames of all employees (Aliasing)
SELECT first_name AS forename, last_name AS surname
FROM employee;

-- Find out all the different genders
SELECT DISTINCT sex
FROM employee;
````

## 2\. Ordering & Limiting Data

```sql
-- Find all employees ordered by salary
-- You can use ASC (Ascending) or DESC (Descending)
SELECT *
FROM employee
ORDER BY salary DESC;

-- Find all employees ordered by sex then name
SELECT *
FROM employee
ORDER BY sex, first_name, last_name;

-- Find the first 5 employees in the table
SELECT *
FROM employee
LIMIT 5;
```

## 3\. Filtering with WHERE

```sql
-- Find all male employees
SELECT *
FROM employee
WHERE sex = 'M';

-- Find all employees at branch 2
SELECT *
FROM employee
WHERE branch_id = 2;

-- Find all employee's ids and names who were born after 1969
SELECT emp_id, first_name, last_name
FROM employee
WHERE birth_day >= '1970-01-01';

-- Find all female employees at branch 2
SELECT *
FROM employee
WHERE branch_id = 2 AND sex = 'F';

-- Find all employees who are female & born after 1969 OR who make over 80000
SELECT *
FROM employee
WHERE (birth_day >= '1970-01-01' AND sex = 'F') OR salary > 80000;

-- Find all employees born between 1970 and 1975
SELECT *
FROM employee
WHERE birth_day BETWEEN '1970-01-01' AND '1975-01-01';

-- Find all employees named Jim, Michael, Johnny or David
SELECT *
FROM employee
WHERE first_name IN ('Jim', 'Michael', 'Johnny', 'David');
```
