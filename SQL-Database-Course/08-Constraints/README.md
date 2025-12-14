# Lesson 8: Constraints


Constraints are rules enforced on data columns in a table. They are used to limit the type of data that can go into a table, ensuring the accuracy and reliability of the data.


## 1. Common Constraints

* **NOT NULL:** Ensures that a column cannot have a `NULL` value.
* **UNIQUE:** Ensures that all values in a column are different.
* **DEFAULT:** Sets a default value for a column when no value is specified.
* **AUTO_INCREMENT:** Automatically generates a unique number for a new record (commonly used for Primary Keys).

> **Tip:** If you wanted to ensure every major was unique, you would use this line while creating the table:
> `major VARCHAR(20) UNIQUE`

## 2. Table Setup with Constraints

Here we create a table that utilizes several constraints at once to automate data entry and prevent errors.

```sql
CREATE TABLE student (
    student_id INT AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL,
    major VARCHAR(20) DEFAULT 'undefined',
    PRIMARY KEY(student_id)
);
````

## 3\. Testing Constraints (Data Insertion)

Notice how we don't need to specify the `student_id` because of `AUTO_INCREMENT`, and if we skip the `major`, it defaults to 'undefined'.

```sql
-- View table before insertion
SELECT * FROM student;

-- 1. Insert name only (ID auto-increments to 1, Major defaults to 'undefined')
INSERT INTO student(name) VALUES('Jack');

-- 2. Insert name and major (ID auto-increments to 2)
INSERT INTO student(name, major) VALUES('Kate', 'Sociology');

-- 3. Columns in different order (ID auto-increments to 3)
INSERT INTO student(major, name) VALUES('English', 'Claire');

-- 4. Duplicate name allowed (ID auto-increments to 4)
INSERT INTO student(name, major) VALUES('Jack', 'Biology');

-- 5. Another standard insert (ID auto-increments to 5)
INSERT INTO student(name, major) VALUES('Mike', 'Computer Science');
```

## 4\. Cleanup/Delete Table

```sql
DROP TABLE student;
```
