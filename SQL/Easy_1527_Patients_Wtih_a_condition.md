# 1978. Employees Whose Manager Left the Company (Easy)

Link to question: https://leetcode.com/problems/patients-with-a-condition/description/?envType=study-plan-v2&envId=top-sql-50

Difficulty: Easy

### Description

Table: Patients


| Column Name  | Type    |
|--------------|---------|
| patient_id   | int     |
| patient_name | varchar |
| conditions   | varchar |

patient_id is the primary key (column with unique values) for this table.\
'conditions' contains 0 or more code separated by spaces. \
This table contains information of the patients in the hospital.
 

Write a solution to find the patient_id, patient_name, and conditions of the patients who have Type I\
Diabetes. Type I Diabetes always starts with DIAB1 prefix.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: \
Patients table:

| patient_id | patient_name | conditions   |
|------------|--------------|--------------|
| 1          | Daniel       | YFEV COUGH   |
| 2          | Alice        |              |
| 3          | Bob          | DIAB100 MYOP |
| 4          | George       | ACNE DIAB100 |
| 5          | Alain        | DIAB201      |

Output: 

| patient_id | patient_name | conditions   |
|------------|--------------|--------------|
| 3          | Bob          | DIAB100 MYOP |
| 4          | George       | ACNE DIAB100 | 

Explanation: Bob and George both have a condition that starts with DIAB1.

### SQL Schema
Create table If Not Exists Patients (patient_id int, patient_name varchar(30), conditions varchar(100))\
Truncate table Patients\
insert into Patients (patient_id, patient_name, conditions) values ('1', 'Daniel', 'YFEV COUGH')\
insert into Patients (patient_id, patient_name, conditions) values ('2', 'Alice', '')\
insert into Patients (patient_id, patient_name, conditions) values ('3', 'Bob', 'DIAB100 MYOP')\
insert into Patients (patient_id, patient_name, conditions) values ('4', 'George', 'ACNE DIAB100')\
insert into Patients (patient_id, patient_name, conditions) values ('5', 'Alain', 'DIAB201')

### Solution

```sql

-- Solution 1: Using REGEXP

SELECT
    patient_id,
    patient_name,
    conditions
FROM
    patients
WHERE conditions REGEXP '^DIAB1.*'
OR conditions REGEXP '.* DIAB1.*';


-- Solution 2: Using LIKE

SELECT
    patient_id,
    patient_name,
    conditions
FROM
    patients
WHERE conditions LIKE 'DIAB1%'
OR conditions LIKE '% DIAB1%';
