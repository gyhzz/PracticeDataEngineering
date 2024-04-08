# 602. Friend Requests II: Who has the Most Friends?

Link to question: https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends/description/?envType=study-plan-v2&envId=top-sql-50

### Description

Table: RequestAccepted


| Column Name    | Type    |
|----------------|---------|
| requester_id   | int     |
| accepter_id    | int     |
| accept_date    | date    |

(requester_id, accepter_id) is the primary key (combination of columns with unique values) for this table.\
This table contains the ID of the user who sent the request, the ID of the user who received the request, and the date when the request was accepted.
 

Write a solution to find the people who have the most friends and the most friends number.

The test cases are generated so that only one person has the most friends.

The result format is in the following example.

 

Example 1:

Input: \
RequestAccepted table:

| requester_id | accepter_id | accept_date |
|--------------|-------------|-------------|
| 1            | 2           | 2016/06/03  |
| 1            | 3           | 2016/06/08  |
| 2            | 3           | 2016/06/08  |
| 3            | 4           | 2016/06/09  |

Output: 

| id | num |
|----|-----|
| 3  | 3   |

Explanation: \
The person with id 3 is a friend of people 1, 2, and 4, so he has three friends in total, which is the most number than any others.
 

Follow up: In the real world, multiple people could have the same most number of friends. Could you find all these people in this case?

### SQL Schema
Create table If Not Exists RequestAccepted (requester_id int not null, accepter_id int null, accept_date date null)\
Truncate table RequestAccepted\
insert into RequestAccepted (requester_id, accepter_id, accept_date) values ('1', '2', '2016/06/03')\
insert into RequestAccepted (requester_id, accepter_id, accept_date) values ('1', '3', '2016/06/08')\
insert into RequestAccepted (requester_id, accepter_id, accept_date) values ('2', '3', '2016/06/08')\
insert into RequestAccepted (requester_id, accepter_id, accept_date) values ('3', '4', '2016/06/09')

### Solution

```sql

-- Solution 1: 

# Write your MySQL query statement below

WITH total_counts AS (
    SELECT
        id,
        SUM(r_count) AS number
    FROM
        (SELECT
            requester_id AS id,
            COUNT(*) AS r_count
        FROM requestaccepted
        GROUP BY requester_id
        UNION ALL
        SELECT
            accepter_id AS id,
            COUNT(*) AS r_count
        FROM requestaccepted
        GROUP BY accepter_id) a
    GROUP BY id)

SELECT
    id,
    number AS num
FROM total_counts
WHERE number = (SELECT MAX(number) FROM total_counts);
