# Lesson 7: Inserting Data


This lesson covers how to add records to your table using the `INSERT` statement. We will also look at how to handle cases where you might not have data for every single column.


## 1. Table Setup

First, we ensure the table exists with a Primary Key defined.

```sql
CREATE TABLE student (
    student_id INT,
    name VARCHAR(20),
    major VARCHAR(20),
    PRIMARY KEY(student_id)
);
````

## 2\. Inserting Data

### Option A: Inserting All Values

If you have data for every column, you can simply list the values in the order the columns were defined.

```sql
INSERT INTO student VALUES(1, 'Jack', 'Biology');
INSERT INTO student VALUES(2, 'Kate', 'Sociology');
```

### Option B: Inserting Partial Values

If you only have data for specific columns (e.g., you don't know the student's major yet), you must specify which columns you are filling. The unspecified columns will be set to `NULL`.

```sql
-- Inserting only ID and Name; Major will be NULL
INSERT INTO student(student_id, name) VALUES(3, 'Claire');
INSERT INTO student(student_id, name) VALUES(4, 'Jack');
```

## 3\. Verifying Data

Use the `SELECT` statement to see the rows you just added.

```sql
SELECT * FROM student;
```
