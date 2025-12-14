# Lesson 7 (Inserting Data)


```sql
CREATE TABLE student (
    student_id INT,
    name VARCHAR(20),
    major VARCHAR(20),
    PRIMARY KEY(student_id)
);
````

`SELECT * FROM student;`

```sql
INSERT INTO student VALUES(1, 'Jack', 'Biology');
INSERT INTO student VALUES(2, 'Kate', 'Sociology');

INSERT INTO student(student_id, name) VALUES(3, 'Claire');
INSERT INTO student(student_id, name) VALUES(4, 'Claire');
```
