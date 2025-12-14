# Lesson 17: Joins


Joins are used to combine rows from two or more tables based on a related column between them.


## 1. Setup: Adding Data

First, we add a new branch called "Buffalo" that currently has **no manager**. This helps illustrate how different joins handle missing data.

```sql
-- Add the extra branch
INSERT INTO branch VALUES(4, 'Buffalo', NULL, NULL);
````

## 2\. Inner Join

An `INNER JOIN` (or just `JOIN`) returns records that have matching values in **both** tables.

  * *Result:* Only lists employees who are also branch managers.

<!-- end list -->

```sql
SELECT employee.emp_id, employee.first_name, branch.branch_name
FROM employee
JOIN branch
ON employee.emp_id = branch.mgr_id;
```

## 3\. Left Join

A `LEFT JOIN` returns **all** records from the left table (`employee`), and the matched records from the right table (`branch`).

  * *Result:* Lists **every** employee. If they are not a manager, the branch name will be `NULL`.

<!-- end list -->

```sql
SELECT employee.emp_id, employee.first_name, branch.branch_name
FROM employee
LEFT JOIN branch
ON employee.emp_id = branch.mgr_id;
```

## 4\. Right Join

A `RIGHT JOIN` returns **all** records from the right table (`branch`), and the matched records from the left table (`employee`).

  * *Result:* Lists **every** branch. Since "Buffalo" has no manager, the employee name will be `NULL`.

<!-- end list -->

```sql
SELECT employee.emp_id, employee.first_name, branch.branch_name
FROM employee
RIGHT JOIN branch
ON employee.emp_id = branch.mgr_id;
```
