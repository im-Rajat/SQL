# Lesson 8 (Constraints)


For Unique element in every column : (Use this while creating a table)

```sql
--major VARCHAR(20) UNIQUE,
````

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

DROP TABLE student;
```
