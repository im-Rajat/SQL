# Lesson 16: Unions


The `UNION` operator allows you to combine the results of multiple `SELECT` statements into a single result set.


> **Rules for Union:**
> 1. You must have the same number of columns in both statements.
> 2. The columns must have similar data types (e.g., both are text or both are numbers).
> 3. The columns in the result set will take the name of the *first* SELECT statement.

## 1. Single Column Union

Merging two lists of names into one column.

```sql
-- Find a list of employee and branch names
-- We alias the column in the first SELECT statement to name the final result.
SELECT employee.first_name AS Employee_Branch_Names
FROM employee
UNION
SELECT branch.branch_name
FROM branch;
````

## 2\. Multi-Column Union

Merging lists that contain multiple data points (e.g., Name and Branch ID). Note that both SELECT statements request exactly two columns.

```sql
-- Find a list of all clients & branch suppliers' names
SELECT client.client_name AS Non_Employee_Entities, client.branch_id AS Branch_ID
FROM client
UNION
SELECT branch_supplier.supplier_name, branch_supplier.branch_id
FROM branch_supplier;
```
