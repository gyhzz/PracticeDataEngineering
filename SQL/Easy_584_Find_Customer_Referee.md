# 584. Find Customer Referee (Easy)

Link to question: https://leetcode.com/problems/find-customer-referee/description/?envType=study-plan-v2&envId=top-sql-50

Difficulty: Easy

### Description

Table: Customer


| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |
| referee_id  | int     |

In SQL, id is the primary key column for this table.\
Each row of this table indicates the id of a customer, their name, and the id of the customer who referred them.
 

Find the names of the customer that are not referred by the customer with id = 2.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: \
Customer table:

| id | name | referee_id |
|----|------|------------|
| 1  | Will | null       |
| 2  | Jane | null       |
| 3  | Alex | 2          |
| 4  | Bill | null       |
| 5  | Zack | 1          |
| 6  | Mark | 2          |

Output: 

| name |
|------|
| Will |
| Jane |
| Bill |
| Zack |



### SQL Schema
Create table If Not Exists Customer (id int, name varchar(25), referee_id int)\
Truncate table Customer\
insert into Customer (id, name, referee_id) values ('1', 'Will', 'None')\
insert into Customer (id, name, referee_id) values ('2', 'Jane', 'None')\
insert into Customer (id, name, referee_id) values ('3', 'Alex', '2')\
insert into Customer (id, name, referee_id) values ('4', 'Bill', 'None')\
insert into Customer (id, name, referee_id) values ('5', 'Zack', '1')\
insert into Customer (id, name, referee_id) values ('6', 'Mark', '2')

### What's the trick? (Spoiler)

Simple filter question, but take note that when filtering for equality or inequality, null results will not be returned. In this case, we want the null results which indicate customers not referred by anyone, so we need to add an additional clause where referee_id IS NULL.

### Solution

```sql

-- Solution

SELECT
    name
FROM customer
WHERE referee_id <> 2
OR referee_id IS NULL;
