# Lesson 19: On Delete


When a row is deleted from a parent table (like `employee`), we need to decide what happens to the rows in other tables that reference it (foreign keys).


## 1. On Delete Set Null

If a foreign key row is deleted, the corresponding value in this table gets set to `NULL`. The row itself remains.
* **Use Case:** When the data is still useful even without the link (e.g., a branch still exists even if the manager is fired).

```sql
-- Definition: If the manager (employee) is deleted, set mgr_id to NULL
CREATE TABLE branch (
  branch_id INT PRIMARY KEY,
  branch_name VARCHAR(40),
  mgr_id INT,
  mgr_start_date DATE,
  FOREIGN KEY(mgr_id) REFERENCES employee(emp_id) ON DELETE SET NULL
);

-- Example:
DELETE FROM employee
WHERE emp_id = 102;

-- Result: The branch row still exists, but mgr_id is now NULL.
SELECT * FROM branch;
SELECT * FROM employee;
````

## 2\. On Delete Cascade

If a foreign key row is deleted, the corresponding row in *this* table is also deleted.

  * **Use Case:** When the data is useless without the link (e.g., a "branch supplier" cannot exist if the branch itself no longer exists).

<!-- end list -->

```sql
-- Definition: If the branch is deleted, delete this supplier row too
CREATE TABLE branch_supplier (
  branch_id INT,
  supplier_name VARCHAR(40),
  supply_type VARCHAR(40),
  PRIMARY KEY(branch_id, supplier_name),
  FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE CASCADE
);

-- Example:
DELETE FROM branch
WHERE branch_id = 2;

-- Result: The supplier rows for Branch 2 are automatically deleted.
SELECT * FROM branch_supplier;
```
