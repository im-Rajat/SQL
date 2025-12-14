# Lesson 18: Nested Queries


Nested queries (or subqueries) are queries embedded inside another query. They are often used in the `WHERE` clause to filter results based on logic that requires a separate lookup steps.

## 1. Using IN with Subqueries

This is useful when the inner query returns a list of values.

```sql
-- Find names of all employees who have sold over 50,000
SELECT employee.first_name, employee.last_name
FROM employee
WHERE employee.emp_id IN (
    SELECT works_with.emp_id
    FROM works_with
    WHERE works_with.total_sales > 50000
);
````

## 2\. Using Equality with Subqueries

This is used when the inner query returns exactly **one** value (like a specific ID).

```sql
-- Find all clients who are handled by the branch that Michael Scott manages
-- Scenario 1: Assume you know Michael's ID (102)
SELECT client.client_id, client.client_name
FROM client
WHERE client.branch_id = (
    SELECT branch.branch_id
    FROM branch
    WHERE branch.mgr_id = 102
);
```

## 3\. Deeply Nested Queries

You can nest a query inside a query inside a query. Here, we find the client -\> based on branch -\> based on manager's name.

```sql
-- Find all clients who are handled by the branch that Michael Scott manages
-- Scenario 2: Assume you DON'T know Michael's ID
SELECT client.client_id, client.client_name
FROM client
WHERE client.branch_id = (
    SELECT branch.branch_id
    FROM branch
    WHERE branch.mgr_id = (
        SELECT employee.emp_id
        FROM employee
        WHERE employee.first_name = 'Michael' AND employee.last_name = 'Scott'
        LIMIT 1
    )
);
```

## 4\. Complex Filtering

Combining standard conditions (`AND`) with subqueries.

```sql
-- Find the names of employees who work with clients handled by the Scranton branch
SELECT employee.first_name, employee.last_name
FROM employee
WHERE employee.branch_id = 2
AND employee.emp_id IN (
    SELECT works_with.emp_id
    FROM works_with
);
```

## 5\. Subqueries in the FROM Clause

Sometimes you need to perform a calculation (like a Sum) first, treat that result as a temporary table, and *then* filter it.

```sql
-- Find the names of all clients who have spent more than 100,000 dollars
SELECT client.client_name
FROM client
WHERE client.client_id IN (
    SELECT client_id
    FROM (
        SELECT SUM(works_with.total_sales) AS totals, client_id
        FROM works_with
        GROUP BY client_id
    ) AS total_client_sales
    WHERE totals > 100000
);
```
