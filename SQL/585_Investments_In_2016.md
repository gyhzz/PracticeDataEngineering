# 585. Investments in 2016

Link to question: https://leetcode.com/problems/investments-in-2016/description/?envType=study-plan-v2&envId=top-sql-50

### Description

Table: Insurance


| Column Name | Type  |
|-------------|-------|
| pid         | int   |
| tiv_2015    | float |
| tiv_2016    | float |
| lat         | float |
| lon         | float |

pid is the primary key (column with unique values) for this table.\
Each row of this table contains information about one policy where:\
pid is the policyholder's policy ID.\
tiv_2015 is the total investment value in 2015 and tiv_2016 is the total investment value in 2016.\
lat is the latitude of the policy holder's city. It's guaranteed that lat is not NULL.\
lon is the longitude of the policy holder's city. It's guaranteed that lon is not NULL.
 

Write a solution to report the sum of all total investment values in 2016 tiv_2016, for all policyholders who:

have the same tiv_2015 value as one or more other policyholders, and\
are not located in the same city as any other policyholder (i.e., the (lat, lon) attribute pairs must be unique).\
Round tiv_2016 to two decimal places.

The result format is in the following example.

 

Example 1:

Input: \
Insurance table:

| pid | tiv_2015 | tiv_2016 | lat | lon |
|-----|----------|----------|-----|-----|
| 1   | 10       | 5        | 10  | 10  |
| 2   | 20       | 20       | 20  | 20  |
| 3   | 10       | 30       | 20  | 20  |
| 4   | 10       | 40       | 40  | 40  |

Output: 

| tiv_2016 |
|----------|
| 45.00    |

Explanation: \
The first record in the table, like the last record, meets both of the two criteria.\
The tiv_2015 value 10 is the same as the third and fourth records, and its location is unique.

The second record does not meet any of the two criteria. Its tiv_2015 is not like any other policyholders and its location is the same as the third record, which makes the third record fail, too.\
So, the result is the sum of tiv_2016 of the first and last record, which is 45.

### SQL Schema
Create Table If Not Exists Insurance (pid int, tiv_2015 float, tiv_2016 float, lat float, lon float)\
Truncate table Insurance\
insert into Insurance (pid, tiv_2015, tiv_2016, lat, lon) values ('1', '10', '5', '10', '10')\
insert into Insurance (pid, tiv_2015, tiv_2016, lat, lon) values ('2', '20', '20', '20', '20')\
insert into Insurance (pid, tiv_2015, tiv_2016, lat, lon) values ('3', '10', '30', '20', '20')\
insert into Insurance (pid, tiv_2015, tiv_2016, lat, lon) values ('4', '10', '40', '40', '40')

### Solution

```sql

-- Solution 1

WITH same_tiv_2015 AS (
    SELECT
        tiv_2015
    FROM insurance
    GROUP BY tiv_2015
    HAVING COUNT(*) > 1
)

, unique_loc AS (
    SELECT
        lat,
        lon
    FROM insurance
    GROUP BY lat, lon
    HAVING COUNT(*) = 1
)

SELECT
    ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM insurance
WHERE tiv_2015 IN (SELECT tiv_2015 FROM same_tiv_2015)
AND (lat, lon) IN (SELECT lat, lon FROM unique_loc);
