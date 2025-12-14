# Lesson 20: Triggers


A **Trigger** is a block of SQL code that automatically executes ("triggers") when a specific event (like INSERT, UPDATE, or DELETE) occurs on a specific table.


## 1. The Concept of Delimiters

> **Note:** `DELIMITER` is used to change the standard end-of-line symbol (`;`) to something else (like `$$`).
>
> **Why?** Because a Trigger contains multiple lines of code that end with `;`. If we didn't change the delimiter, MySQL would think the first `;` inside the trigger was the end of the whole command. We change it to `$$` so MySQL reads the whole block as one command, then we switch it back to `;`.

## 2. Setup

First, we need a place to log the results of our triggers.

```sql
CREATE TABLE trigger_test (
     message VARCHAR(100)
);
````

## 3\. Example 1: Basic Trigger

*Action:* Every time an employee is added, add the message "added new employee" to our test table.

```sql
DELIMITER $$
CREATE
    TRIGGER my_trigger BEFORE INSERT
    ON employee
    FOR EACH ROW BEGIN
        INSERT INTO trigger_test VALUES('added new employee');
    END$$
DELIMITER ;

-- Test the trigger
INSERT INTO employee
VALUES(109, 'Oscar', 'Martinez', '1968-02-19', 'M', 69000, 106, 3);

-- Check result
SELECT * FROM trigger_test;
```

## 4\. Example 2: Using the 'NEW' Keyword

*Action:* Log the **first name** of the person being inserted. `NEW.column_name` allows you to access the specific data being added.

```sql
-- (Necessary step: Remove the old trigger to create a new one with the same name)
DROP TRIGGER IF EXISTS my_trigger;

DELIMITER $$
CREATE
    TRIGGER my_trigger BEFORE INSERT
    ON employee
    FOR EACH ROW BEGIN
        INSERT INTO trigger_test VALUES(NEW.first_name);
    END$$
DELIMITER ;

-- Test the trigger
INSERT INTO employee
VALUES(110, 'Kevin', 'Malone', '1978-02-19', 'M', 69000, 106, 3);

-- Check result
SELECT * FROM trigger_test;
```

## 5\. Example 3: Conditional Triggers (IF/ELSE)

*Action:* Log a different message depending on the gender of the new employee.

```sql
-- Remove the previous trigger
DROP TRIGGER IF EXISTS my_trigger;

DELIMITER $$
CREATE
    TRIGGER my_trigger BEFORE INSERT
    ON employee
    FOR EACH ROW BEGIN
         IF NEW.sex = 'M' THEN
               INSERT INTO trigger_test VALUES('added male employee');
         ELSEIF NEW.sex = 'F' THEN
               INSERT INTO trigger_test VALUES('added female employee');
         ELSE
               INSERT INTO trigger_test VALUES('added other employee');
         END IF;
    END$$
DELIMITER ;

-- Test the trigger
INSERT INTO employee
VALUES(111, 'Pam', 'Beesly', '1988-02-19', 'F', 69000, 106, 3);

-- Check result
SELECT * FROM trigger_test;
```

## 6\. Cleanup

```sql
-- Delete the trigger when finished
DROP TRIGGER my_trigger;
```
