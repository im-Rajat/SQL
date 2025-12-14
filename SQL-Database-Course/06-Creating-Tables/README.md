# Lesson 6: Creating Tables


> **Note:** Whenever we are creating a database, the first thing we do is define a schema, create all the different tables, and then we start inserting data.


## 1. Creating a Table

This command establishes the structure of your data by defining columns and their data types.

```sql
-- Create the Student table
CREATE TABLE student (
    student_id INT PRIMARY KEY,
    name VARCHAR(20),
    major VARCHAR(20)
);
````

## 2\. Managing Table Structure

Once a table is created, you can view its details, modify its columns, or delete it entirely.

```sql
-- View the table structure (columns, types, keys)
DESCRIBE student;

-- Add a new column (GPA)
-- DECIMAL(4, 2) means 4 total digits, 2 after the decimal (e.g., 4.00)
ALTER TABLE student ADD gpa DECIMAL(4, 2);

-- Remove a column
ALTER TABLE student DROP COLUMN gpa;

-- Delete the table entirely
DROP TABLE student;
```
