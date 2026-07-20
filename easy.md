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

## 7. Product Sales Analysis I

### Problem Description
Write a solution to report the product_name, year, price for each sale_id in the Sales table.
Return the result table in any order.

### Schema
Sales
| Column Name | Type |
| --- | --- |
| sale_id | int |
| product_id | int |
| year | int |
| quantity | int |
| price | int |

Product
| Column Name | Type |
| --- | --- |
| product_id | int |
| product_name | varchar |


### Sales table:
| sale_id | product_id | year | quantity | price |
| --- | --- | --- | --- | --- |
| 1 | 100 | 2008 | 10 | 5000 |
| 2 | 100 | 2009 | 12 | 5000 |
| 7 | 200 | 2011 | 15 | 9000 |

### Product table:
| product_id | product_name |
| --- | --- |
| 100 | Nokia |
| 200 | Apple |
| 300 | Samsung |


### Output table:
```text
+--------------+------+-------+
| product_name | year | price |
+--------------+------+-------+
| Nokia        | 2008 | 5000  |
| Nokia        | 2009 | 5000  |
| Apple        | 2011 | 9000  |
+--------------+------+-------|
```
## Solution
```sql
SELECT  Product.product_name, Sales.year, Sales.price
FROM Sales
LEFT JOIN Product
ON  Sales.product_id = Product.product_id;
```
Join Product table at the left of Sales table to produce output needed.

## 8. Customer Who Visited but Did Not Make Any Transactions

### Problem Description
Write a solution to find the IDs of the users who visited without making any transactions and the number of times they made these types of visits.
Return the result table in any order.

### Schema
Visit
| Column Name | Type |
| --- | --- |
| visit_id | int |
| customer_id | int |

Transactions
| Column Name | Type |
| --- | --- |
| transaction_id | int |
| visit_id | int |
| amount | int |


### Visits table:
| visit_id | customer_id |
| --- | --- |
| 1 | 23 |
| 2 | 9 |
| 4 | 30 |
| 5 | 54 |
| 6 | 96 |
| 7| 54 |
| 8 | 54 |

### Transactions table:
| transaction_id | visit_id | amount |
| --- | --- | --- |
| 2 | 5 | 310 |
| 3 | 5 | 300 |
| 9 | 5 | 200 |
| 12 | 1 | 910 |
| 13 | 2 | 970 |


### Output table:
```text
+-------------+----------------+
| customer_id | count_no_trans |
+-------------+----------------+
| 54          | 2              |
| 30          | 1              |
| 96          | 1              |
+-------------+----------------|
```
## Solution
```sql
SELECT  Visits.customer_id, COUNT(Visits.visit_id) AS count_no_trans
FROM Visits
LEFT JOIN Transactions
ON  Visits.customer_id = Transactions.customer_id;
GROUP BY Visits.customer_id;
```
Since we wanted to search for customer that visits without transactions, we need to left join Transactions to Visits in order to keep all rows even without matches on corresponding table.

## 9. Rising Temperature

### Problem Description
Write a solution to find all dates id with higher temperatures compared to its previous dates (yesterday)
Return the result table in any order.

### Schema
Weather
| Column Name | Type |
| --- | --- |
| id | int |
| recordDate | date |
| temperature | int |

### Weather table:
| id | recordDate | temperature |
| --- | --- | --- |
| 1 | 2015-01-01 | 10 |
| 2 | 2015-01-02 | 25 |
| 3 | 2015-01-03 | 20 |
| 4 | 2015-01-04 | 30 |


### Output table:
```text
+----+
| id |
+----+
| 2  |
| 4  |
+----|
```
## Solution
```sql
SELECT  w1.id
FROM Weather w1
LEFT JOIN Weather w2
ON  DATEDIFF(w1.recordDate, w2.recordDate) =1
WHERE w1.temperature > w2.temperature;
```
Join Weather table together and match them with different recordDate using DATEDIFF = 1 and cross with first Weather table has higher temp than second Weather table.

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
ROUND(AVG(a2.timestamp - a1.timestamp), 3) AS processing_time
FROM ACTIVITY a1
LEFT JOIN ACTIVITY a2
ON  a1.machine.id = a2.machine.id
AND a1.process_id = a2.process_id
AND a1.activity_type = 'start'
AND a2.activity_type = 'end'
GROUP BY a1.machine_id;
```
join same table and match them with start time at a1 table and end time at a2, join them with same machine_id and process_id then find avg from their difference of timestamps.

## 11. Employee Bonus

### Problem Description
Write a solution to report the name and bonus amount of each employee who satisfies either of the following:

-The employee has a bonus less than 1000
-The employee did not get any bonus.

Return the result table in any order.

### Schema
Employee
| Column Name | Type |
| --- | --- |
| empId | int |
| name | varchar |
| supervisor | int |
| salary | int |

Bonus
| Column Name | Type |
| --- | --- |
| empId | int |
| bonus | int |

### Employee table:
| empId | name | supervisor | salary |
| --- | --- | --- | --- |
| 3 | Brad | null | 4000 |
| 1 | John | 3 | 1000 |
| 2 | Dan | 3 | 2000 |
| 4 | Thomas | 3 | 4000 |

Bonus table:
| empId | bonus |
| --- | --- |
| 2 | 500 |
| 4 | 2000 |

### Output table:
```text
+------+-------+
| name | bonus |
+------+-------+
| Brad | null  |
| John | null  |
| Dan  | 500   |
+------+-------|
```
## Solution
```sql
SELECT  Employee.name, Bonus.bonus
FROM Employee
LEFT JOIN Bonus
ON  Employee.empID = Bonus.empID
WHERE bonus is NULL or bonus < 1000;
```
Keep every rows in Employee table (Employee LEFT JOIN Bonus) and cross them with criteria.

## 12. Students and Examinations

### Problem Description
Write a solution to find the number of times each student attended each exam.

Return the result table ordered by student_id and subject_name.

### Schema
Students
| Column Name | Type |
| --- | --- |
| student_id | int |
| student_name | varchar |

Subjects
| Column Name | Type |
| --- | --- |
| subject_name | varchar |

Examination
| Column Name | Type |
| --- | --- |
| student_id | int |
| subject_name | varchar |

### Students table:
| student_id | student_name |
| --- | --- |
| 1 | Alice |
| 2 | Bob |
| 13 | John |
| 6 | Alex |

Subjects table:
| subject_name |
| --- |
| Math |
Physics |
| Programming |

Examinations table:
| student_id | subject_name |
| --- | --- |
| 1 | Math |
| 1 | Physics |
| 1 | Programming |
| 2 | Programming |
| 1 | Physics |
| 1 | Math |
| 13 | Math |
| 13 | Programming |
| 13 | Physics |
| 2 | Math |
| 1 | Math |

### Output table:
```text
+------------+--------------+--------------+----------------+
| student_id | student_name | subject_name | attended_exams |
+------------+--------------+--------------+----------------+
| 1          | Alice        | Math         | 3              |
| 1          | Alice        | Physics      | 2              |
| 1          | Alice        | Programming  | 1              |
| 2          | Bob          | Math         | 1              |
| 2          | Bob          | Physics      | 0              |
| 2          | Bob          | Programming  | 1              |
| 6          | Alex         | Math         | 0              |
| 6          | Alex         | Physics      | 0              |
| 6          | Alex         | Programming  | 0              |
| 13         | John         | Math         | 1              |
| 13         | John         | Physics      | 1              |
| 13         | John         | Programming  | 1              |
+------------+--------------+--------------+----------------+
```
## Solution
```sql
SELECT  stu.student_id, stu.student_name, sub.subject_name, COUNT(ex.subject_name) AS attended_exams
FROM Students stu
CROSS JOIN Subjects sub
LEFT JOIN Examination ex
ON  stu.student_id = ex.student_id
AND sub.subject_name = ex.subject_name
GROUP BY stu.student_id, stu.student_name, sub.subject_name
ORDER_BY stu.student_id, sub.subject_name;
```
Result should contain all students and subjects. 
1. Draft SELECT command to produce column name needed.
2. Use CROSS JOIN to match Students with every subjects to create all possible combination.
3. LEFT JOIN to Examination using student_id from Students and subject_name from Subjects. This create a list that put everything in place even the NULL ones.
4. GROUP BY will collapse similar rows into 1
5. COUNT(ex.subject_name) will run together with GROUP BY and calculate the appearance of rows.

## 15. Not Boring Movies

### Problem Description
Write a solution to report the movies with an odd-numbered ID and a description that is not "boring".

Return the result table ordered by rating in descending order.

### Schema
Cinema
| Column Name | Type |
| --- | --- |
| id | int |
| movie | varchar |
| description | varchar |
| rating | float |

### Cinema table:
| id | movie  | description | rating |
| --- | --- | -- | -- | -- |
| 1 | War | great 3D | 8.9 |
| 2 | Science | fiction | 8.5 |
| 3 | irish | boring | 6.2 |
| 4 | Ice song | Fantasy | 8.6 |
| 5 | House card | Interesting | 9.1 |


### Output table:
```text
+----+------------+-------------+--------+
| id | movie      | description | rating |
+----+------------+-------------+--------+
| 1  | War        | great 3D    | 8.9    |
| 2  | Science    | fiction     | 8.5    |
| 3  | irish      | boring      | 6.2    |
| 4  | Ice song   | Fantasy     | 8.6    |
| 5  | House card | Interesting | 9.1    |
+----+------------+-------------+--------+
```
## Solution
```sql
SELECT  id, movie, description, rating
FROM Cinema
WHERE description <> 'boring' ABD id % 2 = 1
ORDER_BY rating DESC;
```

## 15. Average Selling Price

### Problem Description
Write a solution to find the average selling price for each product. average_price should be rounded to 2 decimal places. If a product does not have any sold units, its average selling price is assumed to be 0.

Return the result table in any order.

### Schema
Prices
| Column Name | Type |
| --- | --- |
| product_id | int |
| start_date | date |
| end_date | date |
| price | int |

UnitsSold
| Column Name | Type |
| --- | --- |
| product_id | int |
| purchase_date | date |
| units | int |

### Prices table:
| product_id | start_date  | end_date | price |
| --- | --- | -- | -- |
| 1 | 2019-02-17 | 2019-02-28 | 5 |
| 1 | 2019-03-01 | 2019-03-22 | 20 |
| 2 | 2019-02-01 | 2019-02-20 | 15 |
| 2 | 2019-02-21 | 2019-03-31 | 30 |

UnitsSold table:
| product_id | purchase_date  | units |
| --- | --- | -- |
| 1 | 2019-02-17 | 2019-02-28 |
| 1 | 2019-03-01 | 2019-03-22 |
| 2 | 2019-02-01 | 2019-02-20 |
| 2 | 2019-02-21 | 2019-03-31 |

### Output table:
```text
+------------+---------------+
| product_id | average_price |
+------------+---------------+
| 1          | 6.96          |
| 2          | 16.96         |
+------------+---------------+
```
## Solution
```sql
SELECT p.product_id, IFNULL(ROUND(SUM(units * price)/sum(units),2),0) AS average_price
FROM Prices p
LEFT JOIN UnitsSold s
ON p.product_id = u.product_id
AND u.purchase_date BETWEEN start_date AND end_date
GROUP BY product_id;
```

## 16. Project Employee I

### Problem Description
Write a solution that reports the average experience years of all the employees for each project, rounded to 2 digits.

Return the result table in any order.

### Schema
Project
| Column Name | Type |
| --- | --- |
| product_id | int |
| employee_id | int |

Employee
| Column Name | Type |
| --- | --- |
| employee_id | int |
| name | varchar |
| experience_years | int |

### Project table:
| product_id | employee_id |
| --- | --- |
| 1 | 1 |
| 1 | 2 |
| 1 | 3 |
| 2 | 1 |
| 2 | 4 |

### Employee table:
| employee_id | name | experience_years |
| --- | --- | --- |
| 1 | Khaled | 3 |
| 1 | Ali | 2 |
| 2 | John | 1 |
| 2 | Doe | 2 |

### Output table:
```text
+------------+---------------+
| project_id | average_years |
+------------+---------------+
| 1          | 2.00          |
| 2          | 2.50          |
+------------+---------------+
```
## Solution
```sql
SELECT p.project_id, ROUND(AVERAGE(e.experience_years) ,2) AS average_years
FROM Project p
LEFT JOIN Employee e
ON p.employee_id = e.employee_id
GROUP BY product_id;
```
Average can be calculated by using experience_years without defining formula as we already bounded the equation using GROUP BY product_id. SQL will separate data acc to project_id. Then it gathers matching employees. AVG then calculate experience years that is not null and divide the sum with count. 






