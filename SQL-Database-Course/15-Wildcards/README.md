# Lesson 15: Wildcards


Wildcards are special characters used to search for data that matches a specific pattern, rather than an exact match. They are used with the `LIKE` operator.


## 1. Wildcard Definitions

* **`%` (Percent):** Represents zero, one, or multiple characters.
* **`_` (Underscore):** Represents exactly one single character.

## 2. Pattern Matching Queries

```sql
-- Find any clients who are an LLC
-- matches "John Daly Law, LLC", etc.
SELECT *
FROM client
WHERE client_name LIKE '%LLC';

-- Find any branch suppliers who are in the label business
-- matches any name containing " Label"
SELECT *
FROM branch_supplier
WHERE supplier_name LIKE '% Label%';

-- Find any employee born in October
-- Pattern explanation:
-- ____ (4 underscores) = YYYY
-- _    (1 underscore)  = -
-- 10   (The match)     = Month 10
-- %    (Wildcard)      = Any day
SELECT *
FROM employee
WHERE birth_day LIKE '_____10%';

-- Find any clients who are schools
SELECT *
FROM client
WHERE client_name LIKE '%Highschool%';
````
