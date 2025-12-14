# Lesson 9: Update & Delete


This lesson covers how to modify existing data in your tables (`UPDATE`) and how to remove specific rows (`DELETE`).


## 1. Setup: Resetting the Table

First, let's recreate the `student` table and populate it with data so we have something to modify.

```sql
CREATE TABLE student (
    student_id INT AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL,
    major VARCHAR(20) DEFAULT 'undefined',
    PRIMARY KEY(student_id)
);

SELECT * FROM student;

INSERT INTO student(name) VALUES('Jack');
INSERT INTO student(name, major) VALUES('Kate', 'Sociology');
INSERT INTO student(major, name) VALUES('English', 'Claire');
INSERT INTO student(name, major) VALUES('Jack', 'Biology');
INSERT INTO student(name, major) VALUES('Mike', 'Computer Science');
````

## 2\. Updating Data

The `UPDATE` statement is used to modify existing records. You usually use a `WHERE` clause to specify which rows should be updated; otherwise, *all* rows will be updated.

```sql
-- Change 'Biology' majors to 'Bio'
UPDATE student
SET major = 'Bio'
WHERE major = 'Biology';

-- Change the major to 'Bio' for anyone named 'Jack'
UPDATE student
SET major = 'Bio'
WHERE name = 'Jack';

-- Change major to 'Biochemistry' if the current major is 'Bio' OR 'Chemistry'
UPDATE student
SET major = 'Biochemistry'
WHERE major = 'Bio' OR major = 'Chemistry';

-- Change multiple columns at once (Name and Major) for a specific ID
UPDATE student
SET name = 'Tom', major = 'undefined'
WHERE student_id = 1;
```

## 3\. Deleting Data

The `DELETE` statement removes rows from a table. Like `UPDATE`, it is crucial to use a `WHERE` clause unless you intend to wipe the entire table.

```sql
-- Delete a specific student (ID 3)
DELETE FROM student
WHERE student_id = 3;

-- DANGER: Delete everything from the table (keeps the table structure, removes all data)
DELETE FROM student;
```

## 4\. Cleanup

```sql
-- Removes the table structure entirely
DROP TABLE student;
```
