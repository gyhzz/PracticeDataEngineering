# 1661. Average Time of Process per Machine (Easy)

Link to question: https://leetcode.com/problems/average-time-of-process-per-machine/description/?envType=study-plan-v2&envId=top-sql-50

Difficulty: Easy

### Description

Table: Activity


| Column Name    | Type    |
|----------------|---------|
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum    |
| timestamp      | float   |

The table shows the user activities for a factory website.\
(machine_id, process_id, activity_type) is the primary key (combination of columns with unique values) of this table.
machine_id is the ID of a machine.\
process_id is the ID of a process running on the machine with ID machine_id.\
activity_type is an ENUM (category) of type ('start', 'end').\
timestamp is a float representing the current time in seconds.\
'start' means the machine starts the process at the given timestamp and 'end' means the machine ends the process at the given timestamp.\
The 'start' timestamp will always be before the 'end' timestamp for every (machine_id, process_id) pair.
 

There is a factory website that has several machines each running the same number of processes. Write a solution to find the average time each machine takes to complete a process.

The time to complete a process is the 'end' timestamp minus the 'start' timestamp. The average time is calculated by the total time to complete every process on the machine divided by the number of processes that were run.

The resulting table should have the machine_id along with the average time as processing_time, which should be rounded to 3 decimal places.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: \
Activity table:

| machine_id | process_id | activity_type | timestamp |
|------------|------------|---------------|-----------|
| 0          | 0          | start         | 0.712     |
| 0          | 0          | end           | 1.520     |
| 0          | 1          | start         | 3.140     |
| 0          | 1          | end           | 4.120     |
| 1          | 0          | start         | 0.550     |
| 1          | 0          | end           | 1.550     |
| 1          | 1          | start         | 0.430     |
| 1          | 1          | end           | 1.420     |
| 2          | 0          | start         | 4.100     |
| 2          | 0          | end           | 4.512     |
| 2          | 1          | start         | 2.500     |
| 2          | 1          | end           | 5.000     |

Output: 

| machine_id | processing_time |
|------------|-----------------|
| 0          | 0.894           |
| 1          | 0.995           |
| 2          | 1.456           |

Explanation: \
There are 3 machines running 2 processes each.\
Machine 0's average time is ((1.520 - 0.712) | (4.120 - 3.140)) / 2 = 0.894\
Machine 1's average time is ((1.550 - 0.550) | (1.420 - 0.430)) / 2 = 0.995\
Machine 2's average time is ((4.512 - 4.100) | (5.000 - 2.500)) / 2 = 1.456

### SQL Schema
Create table If Not Exists Activity (machine_id int, process_id int, activity_type ENUM('start', 'end'), timestamp float)\
Truncate table Activity\
insert into Activity (machine_id, process_id, activity_type, timestamp) values ('0', '0', 'start', '0.712')\
insert into Activity (machine_id, process_id, activity_type, timestamp) values ('0', '0', 'end', '1.52')\
insert into Activity (machine_id, process_id, activity_type, timestamp) values ('0', '1', 'start', '3.14')\
insert into Activity (machine_id, process_id, activity_type, timestamp) values ('0', '1', 'end', '4.12')\
insert into Activity (machine_id, process_id, activity_type, timestamp) values ('1', '0', 'start', '0.55')\
insert into Activity (machine_id, process_id, activity_type, timestamp) values ('1', '0', 'end', '1.55')\
insert into Activity (machine_id, process_id, activity_type, timestamp) values ('1', '1', 'start', '0.43')\
insert into Activity (machine_id, process_id, activity_type, timestamp) values ('1', '1', 'end', '1.42')\
insert into Activity (machine_id, process_id, activity_type, timestamp) values ('2', '0', 'start', '4.1')\
insert into Activity (machine_id, process_id, activity_type, timestamp) values ('2', '0', 'end', '4.512')\
insert into Activity (machine_id, process_id, activity_type, timestamp) values ('2', '1', 'start', '2.5')\
insert into Activity (machine_id, process_id, activity_type, timestamp) values ('2', '1', 'end', '5')

### What's the trick? (Spoiler)

Calculating the average processing time of each machine is a simple aggregation of all processing times per machine. The tricky part is to get the processing time of each process given the start and end times of each process. 

There are many ways to do this but here we explore 2 ways - using INNER JOIN and using the LAG window function. The join method is straightforward, we want to match records with the same machine_id, process_id, and where the timestamp of 1 table is higher than another. This will give us machine_id, process_id, process start time, and process end time all in 1 record for us to subtract to find the process time of each process. From here, we can do a simple aggregation to find the average process time of each machine using the AVG function and GROUP BY machine_id.

Using the LAG window function, we essentially want to achieve the same result of putting the start times and end times of each process in the same row. By using LAG, we can create a new column and populate it with the value that come before the current row in the sequence we specify. Here, we partition by machine_id and process_id, telling SQL we only want a value that has machine_id and process_id the same as the current row. Then, we tell SQL the order to find the previous value is (ORDER BY timestamp ASC), meaning a timestamp that is smaller than the current. This way, we're able to populate the start time together with the current row which consists of the later time which is the end time.

From here, the steps are the same, finding the process time by subtracting, then performing the same GROUP BY and aggregation.

### Solution

```sql

-- Solution 1: Using LAG window function

WITH start_end_times AS (
    SELECT
        machine_id,
        process_id,
        timestamp AS end_time,
        LAG(timestamp) OVER(PARTITION BY machine_id, process_id ORDER BY timestamp ASC) AS start_time
    FROM activity
),

calc_p_time AS (
    SELECT
        machine_id,
        process_id,
        end_time - start_time AS p_time
    FROM start_end_times
)

SELECT
    machine_id,
    ROUND(AVG(p_time), 3) AS processing_time
FROM calc_p_time
GROUP BY machine_id;

-- Solution 2: Using INNER JOIN

WITH calc_process_time AS (
    SELECT
        a1.machine_id,
        a1.process_id,
        a1.timestamp - a2.timestamp AS process_time
    FROM activity a1
    INNER JOIN activity a2
    ON a1.machine_id = a2.machine_id
    AND a1.process_id = a2.process_id
    AND a1.timestamp > a2.timestamp
)

SELECT
    machine_id,
    ROUND(AVG(process_time), 3) AS processing_time
FROM calc_process_time
GROUP BY machine_id;
