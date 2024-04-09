# 1667. Fix Names in a Table (Easy)

Link to question: https://leetcode.com/problems/fix-names-in-a-table/description/?envType=study-plan-v2&envId=top-sql-50

Difficulty: Easy

### Description

Table: Users


| Column Name    | Type    |
|----------------|---------|
| user_id        | int     |
| name           | varchar |

user_id is the primary key (column with unique values) for this table.\
This table contains the ID and the name of the user. The name consists of only lowercase and uppercase characters.
 

Write a solution to fix the names so that only the first character is uppercase and the rest are lowercase.

Return the result table ordered by user_id.

The result format is in the following example.

 

Example 1:

Input: \
Users table:

| user_id | name  |
|---------|-------|
| 1       | aLice |
| 2       | bOB   |

Output: 

| user_id | name  |
|---------|-------|
| 1       | Alice |
| 2       | Bob   |


### SQL Schema
Create table If Not Exists Users (user_id int, name varchar(40))\
Truncate table Users\
insert into Users (user_id, name) values ('1', 'aLice')\
insert into Users (user_id, name) values ('2', 'bOB')

### Solution

```sql

-- Solution 1

SELECT
    user_id,
    CONCAT(UPPER(SUBSTRING(name, 1, 1)), LOWER(SUBSTRING(name, 2))) AS name
FROM users
ORDER BY user_id;
