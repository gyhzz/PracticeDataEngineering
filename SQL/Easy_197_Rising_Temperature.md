# 197. Rising Temperature (Easy)

Link to question: https://leetcode.com/problems/rising-temperature/description/?envType=study-plan-v2&envId=top-sql-50

Difficulty: Easy

### Description

Table: Weather


| Column Name   | Type    |
|---------------|---------|
| id            | int     |
| recordDate    | date    |
| temperature   | int     |

id is the column with unique values for this table.\
There are no different rows with the same recordDate.\
This table contains information about the temperature on a certain day.
 

Write a solution to find all dates' Id with higher temperatures compared to its previous dates (yesterday).

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: \
Weather table:

| id | recordDate | temperature |
|----|------------|-------------|
| 1  | 2015-01-01 | 10          |
| 2  | 2015-01-02 | 25          |
| 3  | 2015-01-03 | 20          |
| 4  | 2015-01-04 | 30          |

Output: 

| id |
|----|
| 2  |
| 4  |

Explanation: \
In 2015-01-02, the temperature was higher than the previous day (10 -> 25).\
In 2015-01-04, the temperature was higher than the previous day (20 -> 30).



### SQL Schema
Create table If Not Exists Weather (id int, recordDate date, temperature int)\
Truncate table Weather\
insert into Weather (id, recordDate, temperature) values ('1', '2015-01-01', '10')\
insert into Weather (id, recordDate, temperature) values ('2', '2015-01-02', '25')\
insert into Weather (id, recordDate, temperature) values ('3', '2015-01-03', '20')\
insert into Weather (id, recordDate, temperature) values ('4', '2015-01-04', '30')

### What's the trick? (Spoiler)

For this question you need to find a way to compare values from different rows and you can do this using a self join. You can use an INNER JOIN clause to join the table with itself by matching dates where one directly comes after another, and you can have a second join condition where the temperature of the later day record is higher than the earlier day. From this join, only records of 2 cosecutive dates, as well as a higher temperature on the later day is returned.

If having multiple join conditions is confusing, you can also do the temperature check in a WHERE clause instead. The join will return all consequtive dates, and the where clause (WHERE w1.temperature > w2.temperature) will filter for only records that have a higher temperature than the previous day.

Besides join, you can also use the LAG() window function which returns a value that comes before the current in the order your specify. Here, LAG(temperature) will return the temperature from the row where its recordDate comes before the recordDate of the current row in the sequence of (ORDER BY recordDate ASC). This way, you can have the temperature values you need to compare in the same record and you can perform a simple filtering (WHERE temperature > prev_temperature).

Same for the LAG(recordDate), this will return the previous record date in the sequence and is to enforce that for a record to be selected, the recordDate of the current row must equal to the recordDate of the previous sequence + 1 day, effectively previous day.

### Solution

```sql

-- Solution 1: Using LAG() window function

WITH cte AS (
    SELECT
        id,
        recordDate,
        LAG(recordDate) OVER(ORDER BY recordDate ASC) AS prev_date,
        temperature,
        LAG(temperature) OVER(ORDER BY recordDate ASC) AS prev_temp
    FROM weather
)

SELECT
    id
FROM cte
WHERE temperature > prev_temp
AND recordDate = DATE_ADD(prev_date, INTERVAL 1 DAY);

-- Solution 2: Using INNER JOIN

SELECT
    w1.id
FROM weather w1
INNER JOIN weather w2
ON w1.recordDate = DATE_ADD(w2.recordDate, INTERVAL 1 DAY)
AND w1.temperature > w2.temperature;
