# 570. Managers with At Least 5 Direct Reports (Medium)

Link to question: https://leetcode.com/problems/managers-with-at-least-5-direct-reports/description/?envType=study-plan-v2&envId=top-sql-50

Difficulty: Medium

### Description

Table: Employee


| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |
| department  | varchar |
| managerId   | int     |

id is the primary key (column with unique values) for this table.\
Each row of this table indicates the name of an employee, their department, and the id of their manager.\
If managerId is null, then the employee does not have a manager.\
No employee will be the manager of themself.
 

Write a solution to find managers with at least five direct reports.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input:\
Employee table:

| id  | name  | department | managerId |
|-----|-------|------------|-----------|
| 101 | John  | A          | null      |
| 102 | Dan   | A          | 101       |
| 103 | James | A          | 101       |
| 104 | Amy   | A          | 101       |
| 105 | Anne  | A          | 101       |
| 106 | Ron   | B          | 101       |

Output: 

| name |
|------|
| John |


### SQL Schema
Create table If Not Exists Employee (id int, name varchar(255), department varchar(255), managerId int)\
Truncate table Employee\
insert into Employee (id, name, department, managerId) values ('101', 'John', 'A', 'None')\
insert into Employee (id, name, department, managerId) values ('102', 'Dan', 'A', '101')\
insert into Employee (id, name, department, managerId) values ('103', 'James', 'A', '101')\
insert into Employee (id, name, department, managerId) values ('104', 'Amy', 'A', '101')\
insert into Employee (id, name, department, managerId) values ('105', 'Anne', 'A', '101')\
insert into Employee (id, name, department, managerId) values ('106', 'Ron', 'B', '101')

### What's the trick? (Spoiler)
By counting the number of times each manager's ID show up in the managerId column, we can get the IDs of managers that are the managers for more than 5 employees. Then, do a self join to get the names of the managers by joining the managerIds with the id column.

The reason you need to perform a self join to get the name and not just use the name value when getting the managerId is because the names are actually of the employees managed by the manager, not the manager themselves. You need to find what are the names of the employees that have the manager's IDs.

Below are 2 solutions - Aggregating to filter for managers with at least 5 employees under them, then joining the result with the employees table again to get the names, and another solution where the join is performed first, then aggregating the resulting table and filter.

Solution 1 reduces the dataset by filtering before joining and will be especially performant when the number of managers with at least 5 employees is a small fraction of the full dataset. This reduces the number of rows needed to be compared.

Solution 2 performs the join before performing the aggregation and filtering. This is also acceptable especially if you're working with a querying engine that is optimized for joins.

### Solution

```sql

-- Solution 1: Aggregate first then join to get the name

WITH cte AS (
    SELECT
        managerid,
        COUNT(*) AS reports
    FROM employee
    GROUP BY managerid
    HAVING COUNT(*) >= 5
)

SELECT
    e.name
FROM employee e
INNER JOIN cte c
ON e.id = c.managerid;

-- Solution 2: Join firs then aggregate

SELECT
    e1.name
FROM employee e1
INNER JOIN employee e2
ON e1.id = e2.managerId
GROUP BY e2.managerId
HAVING COUNT(*) >= 5;
