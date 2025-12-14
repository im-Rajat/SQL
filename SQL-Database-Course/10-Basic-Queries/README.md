# Lesson 10: Basic Queries


This lesson focuses on extracting specific data from the `student` table using `SELECT`, `ORDER BY`, `LIMIT`, and various filtering operators.


## 1. Selecting and Ordering Data

We can choose specific columns and determine the order in which results appear.

```sql
-- Select specific columns
SELECT name
FROM student;

-- Select with fully qualified table names (useful for joins later)
-- Ordering by name in Descending order (Z-A)
SELECT student.name, student.major
FROM student
ORDER BY name DESC;

-- Order by Major first, then by Student ID (Descending) for ties
SELECT *
FROM student
ORDER BY major, student_id DESC;

-- Limit the results (e.g., Top 2 recent students)
SELECT *
FROM student
ORDER BY student_id DESC
LIMIT 2;
````

## 2\. Filtering Data (WHERE Clause)

The `WHERE` clause filters records that meet a specified condition.

```sql
-- Filter using OR logic
SELECT name, major
FROM student
WHERE major = 'Biology' OR name = 'Kate';

-- Filter using Comparison Operators (> Greater Than)
SELECT *
FROM student
WHERE student_id > 3;

-- Filter using IN (checks if value matches any in a list)
SELECT *
FROM student
WHERE name IN ('Claire', 'Kate', 'Mike');
```

## 3\. Comparison Operators Reference

These are the standard operators used inside the `WHERE` clause.

  * `<` : Less than
  * `>` : Greater than
  * `<=` : Less than or equal to
  * `>=` : Greater than or equal to
  * `=` : Equal to
  * `<>` : Not equal to
  * `AND` : Both conditions must be true
  * `OR` : At least one condition must be true

<!-- end list -->

