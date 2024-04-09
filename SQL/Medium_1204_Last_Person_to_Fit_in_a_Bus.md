# 1204. Last Person to Fit in a Bus (Medium)

Link to question: https://leetcode.com/problems/last-person-to-fit-in-the-bus/?envType=study-plan-v2&envId=top-sql-50

Difficulty: Medium

### Description

Table: Queue

| Column Name | Type    |
|-------------|---------|
| person_id   | int     |
| person_name | varchar |
| weight      | int     |
| turn        | int     |


person_id column contains unique values.\
This table has the information about all people waiting for a bus.\
The person_id and turn columns will contain all numbers from 1 to n, where n is the number of rows in the table.\
turn determines the order of which the people will board the bus, where turn=1 denotes the first person to board and turn=n denotes the last person to board.\
weight is the weight of the person in kilograms.
 

There is a queue of people waiting to board a bus. However, the bus has a weight limit of 1000 kilograms, so there may be some people who cannot board.

Write a solution to find the person_name of the last person that can fit on the bus without exceeding the weight limit. The test cases are generated such that the first person does not exceed the weight limit.

The result format is in the following example.


Example 1:

Input:\
Queue table:\

| person_id | person_name | weight | turn |
|-----------|-------------|--------|------|
| 5         | Alice       | 250    | 1    |
| 4         | Bob         | 175    | 5    |
| 3         | Alex        | 350    | 2    |
| 6         | John Cena   | 400    | 3    |
| 1         | Winston     | 500    | 6    |
| 2         | Marie       | 200    | 4    |

Output: 

| person_name |
|-------------|
| John Cena   |

Explanation: The folowing table is ordered by the turn for simplicity.

| Turn | ID | Name      | Weight | Total Weight |
|------|----|-----------|--------|--------------|
| 1    | 5  | Alice     | 250    | 250          |
| 2    | 3  | Alex      | 350    | 600          |
| 3    | 6  | John Cena | 400    | 1000         | 
| 4    | 2  | Marie     | 200    | 1200         | 
| 5    | 4  | Bob       | 175    | ___          |
| 6    | 1  | Winston   | 500    | ___          |


John Cena is the last to board and Marie will not be able to board as the total weight will exceed 1000kg if Marie boards.

### SQL Schema
Create table If Not Exists Queue (person_id int, person_name varchar(30), weight int, turn int)\
Truncate table Queue\
insert into Queue (person_id, person_name, weight, turn) values ('5', 'Alice', '250', '1')\
insert into Queue (person_id, person_name, weight, turn) values ('4', 'Bob', '175', '5')\
insert into Queue (person_id, person_name, weight, turn) values ('3', 'Alex', '350', '2')\
insert into Queue (person_id, person_name, weight, turn) values ('6', 'John Cena', '400', '3')\
insert into Queue (person_id, person_name, weight, turn) values ('1', 'Winston', '500', '6')\
insert into Queue (person_id, person_name, weight, turn) values ('2', 'Marie', '200', '4')

### Solution

```sql

-- Solution 1

WITH cumm_weight AS (
    SELECT
        person_id,
        person_name,
        weight,
        turn,
        SUM(weight) OVER(ORDER BY turn ASC ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_weight
    FROM queue
)

, ranked_weight AS (
    SELECT
        person_id,
        person_name,
        weight,
        turn,
        running_weight,
        ROW_NUMBER() OVER(ORDER BY running_weight DESC) AS rnk
    FROM cumm_weight
    WHERE running_weight <= 1000
)

SELECT
    person_name
FROM ranked_weight
WHERE rnk = 1;

-- Solution 2

SELECT q1.person_name
FROM queue q1
JOIN queue q2
ON q1.turn >= q2.turn
GROUP BY q1.turn
HAVING SUM(q2.weight) <= 1000
ORDER BY SUM(q2.weight) DESC
LIMIT 1;
