# SQL Interview Questions

This document contains solutions for the Medium, Challenging, and Advanced SQL problems.

> **Note:** SQL syntax can vary slightly between dialects (PostgreSQL, MySQL, SQL Server). Standard ANSI SQL is used where possible, with specific notes for dialect-specific functions (like dates or string splitting).

---

## 💡 Medium Level

### 1. Write a query to find the second highest salary in an employee table.

**Query:**
```sql
SELECT MAX(salary) 
FROM Employees 
WHERE salary < (SELECT MAX(salary) FROM Employees);
```

*Alternatively (using Window Functions):*

```sql
SELECT DISTINCT salary
FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) as rnk
    FROM Employees
) t
WHERE rnk = 2;
```

**Explanation:**
The first method uses a subquery to exclude the maximum salary, then finds the maximum of the remaining values. The second method (preferred in modern SQL) uses `DENSE_RANK()`, which handles ties correctly (e.g., if two people share the top salary, the next highest is still ranked 2).

### 2. Fetch all employees whose names contain the letter "a" exactly twice.

**Query:**

```sql
SELECT * FROM Employees 
WHERE LENGTH(name) - LENGTH(REPLACE(LOWER(name), 'a', '')) = 2;
```

**Explanation:**
We calculate the length of the original name and subtract the length of the name with all 'a's removed. The difference represents the count of the character 'a'. `LOWER()` ensures case-insensitivity.

### 3. How do you retrieve only duplicate records from a table?

**Query:**

```sql
SELECT column_name, COUNT(*)
FROM TableName
GROUP BY column_name
HAVING COUNT(*) > 1;
```

**Explanation:**
We group the data by the column we suspect has duplicates. The `HAVING` clause filters these groups to show only those appearing more than once.

### 4. Write a query to calculate the running total of sales by date.

**Query:**

```sql
SELECT 
    sales_date, 
    sales_amount,
    SUM(sales_amount) OVER (ORDER BY sales_date) as running_total
FROM Sales;
```

**Explanation:**
The window function `SUM(...) OVER (ORDER BY ...)` calculates a cumulative sum that grows with each row, sorted by date.

### 5. Find employees who earn more than the average salary in their department.

**Query:**

```sql
SELECT * FROM Employees e
WHERE salary > (
    SELECT AVG(salary) 
    FROM Employees 
    WHERE department_id = e.department_id
);
```

**Explanation:**
This uses a **correlated subquery**. For every employee row processed in the outer query, the subquery calculates the average salary for that specific employee's department.

### 6. Write a query to find the most frequently occurring value in a column.

**Query:**

```sql
SELECT column_name
FROM TableName
GROUP BY column_name
ORDER BY COUNT(*) DESC
LIMIT 1; 
-- Use TOP 1 for SQL Server
```

**Explanation:**
We group by the value and count occurrences, then order by that count in descending order. Taking the top record gives the mode.

### 7. Fetch records where the date is within the last 7 days from today.

**Query (SQL Server):**

```sql
SELECT * FROM TableName
WHERE date_col >= DATEADD(day, -7, GETDATE());
```

**Query (PostgreSQL/MySQL):**

```sql
SELECT * FROM TableName
WHERE date_col >= CURRENT_DATE - INTERVAL '7 days';
```

**Explanation:**
We filter records where the date column is greater than or equal to the current timestamp minus 7 days.

### 8. Write a query to count how many employees share the same salary.

**Query:**

```sql
SELECT salary, COUNT(*) as employee_count
FROM Employees
GROUP BY salary
HAVING COUNT(*) > 1;
```

**Explanation:**
Similar to finding duplicates, we group by salary and count the employees in each group.

### 9. How do you fetch the top 3 records for each group in a table?

**Query:**

```sql
SELECT * FROM (
    SELECT *, 
           ROW_NUMBER() OVER (PARTITION BY group_col ORDER BY value_col DESC) as rn
    FROM TableName
) t
WHERE rn <= 3;
```

**Explanation:**
We use `ROW_NUMBER()` (or `RANK()`/`DENSE_RANK()` depending on how ties should be handled). `PARTITION BY` restarts the counting for each group, and `ORDER BY` determines which records are "top". We then filter for rank <= 3.

### 10. Retrieve products that were never sold (hint: use LEFT JOIN).

**Query:**

```sql
SELECT p.product_name
FROM Products p
LEFT JOIN Sales s ON p.product_id = s.product_id
WHERE s.sales_id IS NULL;
```

**Explanation:**
A `LEFT JOIN` keeps all records from the left table (Products). If there is no match in the right table (Sales), the columns from Sales will be `NULL`. Filtering for `NULL` isolates the unsold products.

---

## 💡 Challenging Level

### 1. Retrieve customers who made their first purchase in the last 6 months.

**Query:**

```sql
SELECT customer_id
FROM Sales
GROUP BY customer_id
HAVING MIN(purchase_date) >= DATEADD(month, -6, GETDATE()); -- SQL Server syntax
```

**Explanation:**
We group by customer and look at their `MIN(purchase_date)` (their first purchase). If that minimum date is recent (within 6 months), they are new customers.

### 2. How do you pivot a table to convert rows into columns?

**Query (Standard ANSI approach):**

```sql
SELECT 
    department_id,
    SUM(CASE WHEN year = 2023 THEN revenue ELSE 0 END) as Rev_2023,
    SUM(CASE WHEN year = 2024 THEN revenue ELSE 0 END) as Rev_2024
FROM RevenueTable
GROUP BY department_id;
```

**Explanation:**
While some DBs have a `PIVOT` operator, the most portable way is using `CASE` statements inside an aggregate function (`SUM` or `MAX`) to conditionally select data for columns.

### 3. Write a query to calculate the percentage change in sales month-over-month.

**Query:**

```sql
WITH MonthlySales AS (
    SELECT 
        FORMAT(sale_date, 'yyyy-MM') as month, 
        SUM(amount) as total_sales
    FROM Sales
    GROUP BY FORMAT(sale_date, 'yyyy-MM')
)
SELECT 
    month,
    total_sales,
    (total_sales - LAG(total_sales) OVER (ORDER BY month)) * 100.0 / 
    NULLIF(LAG(total_sales) OVER (ORDER BY month), 0) as pct_change
FROM MonthlySales;
```

**Explanation:**
We calculate total sales per month first. Then, `LAG(total_sales)` retrieves the previous month's sales. The formula is `(Current - Previous) / Previous`.

### 4. Find the median salary of employees in a table.

**Query (PostgreSQL/SQL Server):**

```sql
SELECT DISTINCT 
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary) OVER () as median_salary
FROM Employees;
```

**Explanation:**
Calculating a true median is hard in basic SQL. `PERCENTILE_CONT(0.5)` is a window function specifically designed to find the value at the 50th percentile.

### 5. Fetch all users who logged in consecutively for 3 days or more.

**Query:**

```sql
WITH DatedLogins AS (
    SELECT DISTINCT user_id, login_date 
    FROM Logins
),
Groups AS (
    SELECT 
        user_id, 
        login_date, 
        DATEADD(day, -ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date), login_date) as grp
    FROM DatedLogins
)
SELECT user_id 
FROM Groups
GROUP BY user_id, grp
HAVING COUNT(*) >= 3;
```

**Explanation:**
This is a "Gaps and Islands" problem. By subtracting the `ROW_NUMBER` (in days) from the actual date, consecutive dates result in the same constant date value (`grp`). Grouping by this calculated value allows us to count the size of the consecutive "island".

### 6. Write a query to delete duplicate rows while keeping one occurrence.

**Query:**

```sql
WITH CTE AS (
    SELECT *, 
           ROW_NUMBER() OVER (PARTITION BY name, email ORDER BY id) as rn
    FROM Users
)
DELETE FROM CTE WHERE rn > 1;
```

**Explanation:**
We assign a row number to duplicates (partitioning by the columns that define uniqueness). The first occurrence gets `1`, others get `2, 3...`. We delete any row where the number is greater than 1.

### 7. Create a query to calculate the ratio of sales between two categories.

**Query:**

```sql
SELECT 
    SUM(CASE WHEN category = 'Electronics' THEN amount ELSE 0 END) * 1.0 /
    NULLIF(SUM(CASE WHEN category = 'Clothing' THEN amount ELSE 0 END), 0) as ratio
FROM Sales;
```

**Explanation:**
We use conditional aggregation to sum the two specific categories separately, then divide them. `NULLIF` prevents division by zero errors.

### 8. How would you implement a recursive query to generate a hierarchical structure?

**Query (Employee Manager Hierarchy):**

```sql
WITH RECURSIVE Hierarchy AS (
    -- Anchor member: Start with top-level manager
    SELECT id, name, manager_id, 1 as level
    FROM Employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Recursive member: Join back to the hierarchy
    SELECT e.id, e.name, e.manager_id, h.level + 1
    FROM Employees e
    INNER JOIN Hierarchy h ON e.manager_id = h.id
)
SELECT * FROM Hierarchy;
```

**Explanation:**
We use a Common Table Expression (CTE) with `RECURSIVE`. The first part selects the root. The second part joins the original table with the CTE itself to find children of the current rows, repeating until no new rows are found.

### 9. Write a query to find gaps in sequential numbering within a table.

**Query:**

```sql
SELECT id + 1 as gap_start, next_id - 1 as gap_end
FROM (
    SELECT id, LEAD(id) OVER (ORDER BY id) as next_id
    FROM Sequences
) t
WHERE next_id - id > 1;
```

**Explanation:**
We look at the current ID and the `LEAD` (next) ID. If `next_id - current_id` is greater than 1, it means numbers are missing in between them.

### 10. Split a comma-separated string into individual rows using SQL.

**Query (SQL Server):**

```sql
SELECT value 
FROM STRING_SPLIT('Apple,Banana,Orange', ',');
```

**Query (PostgreSQL):**

```sql
SELECT unnest(string_to_array('Apple,Banana,Orange', ','));
```

**Explanation:**
Modern SQL dialects provide built-in functions (`STRING_SPLIT` or `unnest`) to transform a delimited string into a table of rows.

---

## 💡 Advanced Problem-Solving

### 1. Rank products by sales in descending order for each region.

**Query:**

```sql
SELECT 
    region,
    product_name,
    sales,
    DENSE_RANK() OVER (PARTITION BY region ORDER BY sales DESC) as rank
FROM ProductSales;
```

**Explanation:**
`DENSE_RANK()` assigns a rank within each `PARTITION` (Region). We order by sales descending to give the highest sales rank 1.

### 2. Fetch all employees whose salaries fall within the top 10% of their department.

**Query:**

```sql
SELECT * FROM (
    SELECT *,
           PERCENT_RANK() OVER (PARTITION BY department_id ORDER BY salary) as pct
    FROM Employees
) t
WHERE pct >= 0.9;
```

**Explanation:**
`PERCENT_RANK()` calculates the relative rank of a row (from 0 to 1). The top 10% will have a rank of 0.9 or higher.

### 3. Identify orders placed during business hours (e.g., 9 AM to 6 PM).

**Query:**

```sql
SELECT * FROM Orders
WHERE DATEPART(hour, order_time) >= 9 
  AND DATEPART(hour, order_time) < 18;
```

**Explanation:**
We extract the hour part of the timestamp. This assumes "6 PM" means up to 17:59:59. If 18:00 is inclusive, change `< 18` to `<= 18`.

### 4. Write a query to get the count of users active on both weekdays and weekends.

**Query:**

```sql
SELECT user_id
FROM Activity
GROUP BY user_id
HAVING COUNT(DISTINCT CASE WHEN DATEPART(dw, activity_date) IN (1, 7) THEN 1 END) > 0 -- Has Weekend
   AND COUNT(DISTINCT CASE WHEN DATEPART(dw, activity_date) NOT IN (1, 7) THEN 1 END) > 0; -- Has Weekday
```

**Explanation:**
We group by user and check two conditions in the `HAVING` clause. We use conditional counting to ensure the user has at least one record where the day is Sat/Sun (weekend) AND at least one record where it is Mon-Fri. (Note: Day numbers vary by dialect; 1=Sunday, 7=Saturday in standard SQL Server).

### 5. Retrieve customers who made purchases across at least three different categories.

**Query:**

```sql
SELECT customer_id
FROM Sales
GROUP BY customer_id
HAVING COUNT(DISTINCT category_id) >= 3;
```

**Explanation:**
We group by customer and count the *distinct* number of categories associated with their sales. If the count is 3 or more, they meet the criteria.
