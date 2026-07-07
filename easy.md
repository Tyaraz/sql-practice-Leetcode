# Leetcode Easy SQL Questions

## 1. Recyclable and Low Fat Products

### Problem Description
Write a solution to find the ids of products that are both low fat and recyclable.
Return the result table in any order.

### Schema
| Column Name | Type |
| --- | --- |
| product_id | int |
| low_fats | enum |
| recyclable | enum |

### Products table:
| product_id | low_fats | recyclable |
| --- | --- | --- |
| 0 | Y | N |
| 1 | Y | Y |
| 2 | N | Y |
| 3 | Y | Y |
| 4 | N | N |

### Output table:
```text
+------------+
| product_id |
+------------+
| 1          |
| 3          |
+------------+
```
## Solution
```sql
SELECT product_id
FROM Products
WHERE low_fats = 'Y' AND recyclable = 'Y';
```
---

## 2. Find Customer Referee

### Problem Description
Find the name of the customer that are either:
1. referred by any customer with id != 2
2. not referred by any customer
Return the result table in any order.

### Schema
| Column Name | Type |
| --- | --- |
| id | int |
| name | varchar |
| referee_id | int |

### Customer table:
| id | name | referee_id |
| --- | --- | --- |
| 1 | Will | null |
| 2 | Jane | null |
| 3 | Alex | 2 |
| 4 | Bill | null |
| 5 | Zack | 1 |
| 6 | Mark | 2 |

### Output table:
```text
+------+
| name |
+------+
| Will |
| Jane |
| Bill |
| Zack |
+------+
```
## Solution
```sql
SELECT name
FROM Customer
WHERE referee_id != 2 OR referee_id IS NULL;
```
---

## 3. Big Countries

### Problem Description
A country is big if:
-it has an area of at least three million km2 or
-it has a population of at least twenty-five million
Write a solution to find the name, population, and area of the big countries.
Return the result table in any order.

### Schema
| Column Name | Type |
| --- | --- |
| name | varchar |
| continent | varchar |
| area | int |
| population | int |
| gdp | bigint |

### World table:
| name | continent | area | population | gdp |
| --- | --- | --- | --- | --- |
| Afghanistan | Asia | 652230 | 25500100 | 20343000000 |
| Albania | Europe | 28748 | 2831741 | 12960000000 |
| Algeria | Africa | 2381741 | 37100000 | 188681000000 |
| Andorra | Europe | 468 | 78115 | 3712000000 |
| Angola | Africa | 1246700 | 20609294 | 100990000000 |

### Output table:
```text
+-------------+-------------+------+
| name        | population  | area |
+-------------+----------+---------+
| Afghanistan | 25500100 | 6552230 |
| Algeria     | 37100000 | 2381741 |
+-------------+----------+---------|
```
## Solution
```sql
SELECT name, population, area
FROM World
WHERE area >= 3000000 OR population >= 25000000;
```

## 4. Article Views I

### Problem Description
Write a solution to find all the authors that viewed at least one of their own articles.
Return the result table sorted by id in ascending order.

### Schema
| Column Name | Type |
| --- | --- |
| article_id | int |
| author_id | int |
| viewer_id | int |
| view_date | date |

### Views table:
| article_id | author_id | viewer_id | view_date |
| --- | --- | --- | --- |
| 1 | 3 | 5 | 2019-08-01 |
| 1 | 3 | 6 | 2019-08-02 |
| 2 | 7 | 7 | 2019-08-01 |
| 2 | 7 | 6 | 2019-08-02 |
| 4 | 7 | 1 | 2019-07-22 |
| 3 | 4 | 4 | 2019-07-21 |
| 3 | 4 | 4 | 2019-07-21 |


### Output table:
```text
+-----+
| id  |
+-----+
| 4   |
| 7   |
+-----|
```
## Solution
```sql
SELECT DISTINCT author_id as id
FROM Views
WHERE author_id = viewer_id
ORDER BY id ASC;
```

## 5. Invalid Tweets

### Problem Description
Write a solution to find the IDs of the invalid tweets. The tweet is invalid if the number of characters used in the content of the tweet is strictly greater than 15.
Return the result table in any order.

### Schema
| Column Name | Type |
| --- | --- |
| tweet_id | int |
| content | varchar |

### Tweets table:
| tweet_id | content |
| --- | --- |
| 1 | Let us Code |
| 1 | 3 |
| 2 | More than fifteen chars are here! |

### Output table:
```text
+-----+
| tweet_id  |
+-----+
| 2  |
+-----|
```
## Solution
```sql
SELECT  tweet_id
FROM Output
WHERE CHAR_LENGTH(content) > 15;
```

## 6. Replace Employee ID with The Unique Identifier

### Problem Description
Write a solution to show the unique ID of each user. If a user does not have a unique ID, replace it with null.
Return the result table in any order.

### Schema
Employees
| Column Name | Type |
| --- | --- |
| id | int |
| name | varchar |

EmployeeUNI
| Column Name | Type |
| --- | --- |
| id | int |
| unique_id | int |


### Employees table:
| id | name |
| --- | --- |
| 1 | Alice |
| 7 | Bob |
| 11 | Meir |
| 90 | Winston |
| 3 | Jonathan |

### EmployeeUNI table:
| id | unique_id |
| --- | --- |
| 3 | 1 |
| 11 | 2 |
| 90 | 3 |


### Output table:
```text
+-----------+-----------+
| unique_id | name      |
+-----------+-----------+
| null      | Alice     |
| null      | Bob       |
| 2         | Meir      |
| 3         | Winston   |
| 1         | Jonathan  |
+-----------+-----------|
```
## Solution
```sql
SELECT  EmployeeUNI.unique_id, Employee.name
FROM Employees
LEFT JOIN EmployeeUNI
ON  Employees.id = EmployeeUNI.id;
```
Connect Employees table to EmployeeUNI table on the left, with matching unique_id

## 10. Average Time of Process per Machine

### Problem Description
There is a factory website that has several machines each running the same number of processes. Write a solution to find the average time each machine takes to complete a process.

Time to complete a process is the 'end' time minus 'start' time. Average time is calculated by the total time to complete every process fivided by the number of processes that were run.

The resulting table should have the machine_id and average time as processing _time, which should be rounded to 3 decimal places.
Return the result table in any order.

### Schema
Activity
| Column Name | Type |
| --- | --- |
| machine_id | int |
| process_id | int |
| activity_type | enum |
| timestamp | float |


### Activity table:
| machine_id | process_id | activity_type | timestamp |
| --- | --- | --- | --- |
| 0 | 0 | start | 0.712 |
| 0 | 0 | end | 1.520 |
| 0 | 1 | start | 3.140 |
| 0 | 1 | end | 4.120 |
| 1 | 0 | start | 0.550 |
| 1 | 0 | end | 1.550 |
| 1 | 1 | start | 0.430 |
| 1 | 1 | end | 1.420 |
| 2 | 0 | start | 4.100 |
| 2 | 0 | end | 4.512 |
| 2 | 1 | start | 2.500 |
| 2 | 1 | end | 5.000 |


### Output table:
```text
+------------+-----------------+
| machine_id | processing_time |
+------------+-----------------+
| 0          | 0.894           |
| 1          | 0.995           |
| 2          | 1.456           |
+------------+-----------------|
```
## Solution
```sql
SELECT  a1.machine_id,
ROUND(AVG(a2.timestamo - a1.timestamp), 3) AS processing_time
FROM ACTIVITY a1
LEFT JOIN ACTIVITY a2
ON  a1.machine.id = a2.machine.id
AND a1.process_id = a2.process_id
AND a1.activity_type = 'start'
AND a2.activity_type = 'end'
GROUP BY a1.machine_id;
```
summarize start time at a1 table and end time at a2, join them with same machine_id and process_id then find avg from their difference.





