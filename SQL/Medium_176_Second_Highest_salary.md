# 176. Second Highest Salary (Medium)

Link to question: https://leetcode.com/problems/second-highest-salary/description/?envType=study-plan-v2&envId=top-sql-50

Difficulty: Medium

### Description

Table: Employee


| Column Name | Type |
|-------------|------|
| id          | int  |
| salary      | int  |

id is the primary key (column with unique values) for this table.\
Each row of this table contains information about the salary of an employee.
 

Write a solution to find the second highest salary from the Employee table. If there is no second highest salary, return null (return None in Pandas).

The result format is in the following example.

 

Example 1:

Input: \
Employee table:

| id | salary |
|----|--------|
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |

Output: 

| SecondHighestSalary |
|---------------------|
| 200                 |

Example 2:

Input: \
Employee table:

| id | salary |
|----|--------|
| 1  | 100    |

Output: 

| SecondHighestSalary |
|---------------------|
| null                |


### SQL Schema
Create table If Not Exists Employee (id int, salary int)\
Truncate table Employee\
insert into Employee (id, salary) values ('1', '100')\
insert into Employee (id, salary) values ('2', '200')\
insert into Employee (id, salary) values ('3', '300')

### What's the trick? (Spoiler)

It seems simple enough to get the second highest salary by applying a rank ordered by salary in descending order and getting the second rank, but you'll realise you can't get null when you try to filter for rank 2.

You have to figure out what function will be able to return a null value. One way is to use the IFNULL(<result set>, null) function such that if result is empty, you can return null instead. Or, when aggregating an empty result set, SQL returns null by default.

Also, consider edge cases where there could be multiple occurences of highest salary, multiple occurences of second highest salary.

### Solution

```sql

-- Solution 1: Using IFNULL and DENSE_RANK

SELECT IFNULL(
    (SELECT
        salary
    FROM (
        SELECT
            salary,
            DENSE_RANK() OVER(ORDER BY salary DESC) AS rnk
        FROM employee
    ) a
    WHERE rnk = 2
    LIMIT 1), NULL) AS SecondHighestSalary;

-- Solution 2: Using MAX and Subquery

SELECT
    MAX(salary) AS SecondHighestSalary
FROM
    employee
WHERE
    salary < (SELECT MAX(salary) FROM employee);
