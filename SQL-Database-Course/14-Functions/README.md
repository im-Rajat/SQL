# Lesson 14: SQL Functions


This lesson introduces aggregate functions (`COUNT`, `AVG`, `SUM`) to perform calculations on data, as well as the `GROUP BY` statement to categorize those calculations.


## 1. Aggregate Functions

These functions perform a calculation on a set of values and return a single value.

```sql
-- Find the number of employees
-- Note: COUNT(column_name) counts non-NULL values.
SELECT COUNT(super_id)
FROM employee;

-- Find the average of all employee's salaries
SELECT AVG(salary)
FROM employee;

-- Find the sum of all employee's salaries
SELECT SUM(salary)
FROM employee;
````

## 2\. Grouping Data (GROUP BY)

`GROUP BY` allows you to aggregate data based on specific categories (e.g., by gender, by client, or by employee).

```sql
-- Find out how many males and females there are
SELECT COUNT(sex), sex
FROM employee
GROUP BY sex;

-- Find the total sales of each salesman
SELECT SUM(total_sales), emp_id
FROM works_with
GROUP BY emp_id;

-- Find the total amount of money spent by each client
SELECT SUM(total_sales), client_id
FROM works_with
GROUP BY client_id;
```
