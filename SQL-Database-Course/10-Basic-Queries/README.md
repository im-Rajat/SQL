# Lesson 10 (Basic Queries)


```sql
SELECT name
FROM student;

SELECT student.name, student.major
FROM student
ORDER BY name DESC;

SELECT *
FROM student
ORDER BY major, student_id DESC;

SELECT *
FROM student
ORDER BY student_id DESC
LIMIT 2;

SELECT name, major
FROM student
WHERE major = 'Biology' OR name = 'Kate';

-- <, >, <=, >=, =, <>, AND, OR
-- <> is not equal

SELECT *
FROM student
WHERE student_id > 3;

SELECT *
FROM student
WHERE name IN ('Clair', 'Kate', 'Mike');
````
