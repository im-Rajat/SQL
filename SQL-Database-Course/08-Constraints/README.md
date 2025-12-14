# Lesson 8: Constraints

**Constraints** are rules enforced on data columns in a table. They are used to limit the type of data that can go into a table.

### 1. Unique Constraint
To ensure a unique element in every column (use this while creating a table definition):

```sql
-- Example syntax:
-- major VARCHAR(20) UNIQUE,
````

### 2\. Table Creation Example

This example demonstrates `AUTO_INCREMENT`, `NOT NULL`, `DEFAULT`, and `PRIMARY KEY`.

```sql
CREATE TABLE student (
    student_id INT AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL,
    major VARCHAR(20) DEFAULT 'undefined',
    PRIMARY KEY(student_id)
);
```

  * **AUTO\_INCREMENT:** Automatically generates a number (1, 2, 3...) for the ID.
  * **NOT NULL:** The `name` column cannot be left empty.
  * **DEFAULT:** If no `major` is provided, it will automatically be set to 'undefined'.
  * **PRIMARY KEY:** Uniquely identifies each record.

### 3\. Viewing Data

To see all current rows in the table:

```sql
SELECT * FROM student;
```

### 4\. Inserting Data

Different ways to insert data to test the constraints:

```sql
-- 1. Only providing name (ID auto-increments, Major defaults to 'undefined')
INSERT INTO student(name) VALUES('Jack');

-- 2. Standard insertion
INSERT INTO student(name, major) VALUES('Kate', 'Sociology');

-- 3. Swapping order of columns (valid if values match the order)
INSERT INTO student(major, name) VALUES('English', 'Claire');

-- 4. Inserting a duplicate name (Allowed because 'name' is not UNIQUE)
INSERT INTO student(name, major) VALUES('Jack', 'Biology');

-- 5. Standard insertion
INSERT INTO student(name, major) VALUES('Mike', 'Computer Science');
```

### 5\. Deleting the Table

To remove the table from the database:

```sql
DROP TABLE student;
```


### Summary of Changes Made
* **Syntax Highlighting:** Wrapped SQL code in ` ```sql ` blocks so it is easier to read.
* **Spelling:** Corrected `coloumn` to `column`.
* **Definitions:** Added short definitions for `AUTO_INCREMENT`, `NOT NULL`, etc., to make the notes more educational.
* **Comments:** Added comments to the `INSERT` statements to explain what specific behavior (like defaults or column swapping) is being tested.

Would you like me to explain any of the specific constraints (like `FOREIGN KEY` or `CHECK`) that weren't included in your original notes?