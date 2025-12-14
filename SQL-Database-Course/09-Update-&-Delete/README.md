# Lesson 9 (Update & Delete)


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

UPDATE student
SET major = 'Bio'
WHERE major = 'Biology';

UPDATE student
SET major = 'Bio'
WHERE name = 'Jack';

UPDATE student
SET major = 'Biochemistry'
WHERE major = 'Bio' OR major = 'Chemistry';

UPDATE student
SET name = 'Tom', major = 'undefined'
WHERE student_id = 1;

DELETE from student
WHERE student_id = 3;

DELETE from student; -- to delete everything from the table.
````
