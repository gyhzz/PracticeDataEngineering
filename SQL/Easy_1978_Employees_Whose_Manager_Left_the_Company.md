# 1978. Employees Whose Manager Left the Company (Easy)

Link to question: https://leetcode.com/problems/employees-whose-manager-left-the-company/description/?envType=study-plan-v2&envId=top-sql-50

Difficulty: Easy

### Description

Table: Employees


| Column Name | Type     |
|-------------|----------|
| employee_id | int      |
| name        | varchar  |
| manager_id  | int      |
| salary      | int      |

In SQL, employee_id is the primary key for this table.\
This table contains information about the employees, their salary, and the ID of their manager. Some employees do not have a manager (manager_id is null). 
 

Find the IDs of the employees whose salary is strictly less than $30000 and whose manager left the company. When a manager leaves the company, their information is deleted from the Employees table, but the reports still have their manager_id set to the manager that left.

Return the result table ordered by employee_id.

The result format is in the following example.

 

Example 1:

Input:\
Employees table:

| employee_id | name      | manager_id | salary |
|-------------|-----------|------------|--------|
| 3           | Mila      | 9          | 60301  |
| 12          | Antonella | null       | 31000  |
| 13          | Emery     | null       | 67084  |
| 1           | Kalel     | 11         | 21241  |
| 9           | Mikaela   | null       | 50937  |
| 11          | Joziah    | 6          | 28485  |

Output: 

| employee_id |
|-------------|
| 11          |


Explanation:\
The employees with a salary less than $30000 are 1 (Kalel) and 11 (Joziah).\
Kalel's manager is employee 11, who is still in the company (Joziah).\
Joziah's manager is employee 6, who left the company because there is no row for employee 6 as it was deleted.

### SQL Schema
Create table If Not Exists Employees (employee_id int, name varchar(20), manager_id int, salary int)\
Truncate table Employees\
insert into Employees (employee_id, name, manager_id, salary) values ('3', 'Mila', '9', '60301')\
insert into Employees (employee_id, name, manager_id, salary) values ('12', 'Antonella', 'None', '31000')\
insert into Employees (employee_id, name, manager_id, salary) values ('13', 'Emery', 'None', '67084')\
insert into Employees (employee_id, name, manager_id, salary) values ('1', 'Kalel', '11', '21241')\
insert into Employees (employee_id, name, manager_id, salary) values ('9', 'Mikaela', 'None', '50937')\
insert into Employees (employee_id, name, manager_id, salary) values ('11', 'Joziah', '6', '28485')

### Solution

```sql

-- Solution 1

SELECT
    e1.employee_id
FROM employees e1
LEFT JOIN employees e2
ON e1.manager_id = e2.employee_id
WHERE e1.manager_id IS NOT NULL
AND e2.employee_id IS NULL
AND e1.salary < 30000
ORDER BY e1.employee_id;

-- Solution 2

SELECT
    employee_id
FROM employees
WHERE salary < 30000
AND manager_id IS NOT NULL
AND manager_id NOT IN (SELECT employee_id FROM employees)
ORDER BY employee_id;