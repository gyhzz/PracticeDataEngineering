# 1757. Recyclable and Low Fat Products (Easy)

Link to question: https://leetcode.com/problems/recyclable-and-low-fat-products/description/?envType=study-plan-v2&envId=top-sql-50

Difficulty: Easy

### Description

Table: Products


| Column Name | Type    |
|-------------|---------|
| product_id  | int     |
| low_fats    | enum    |
| recyclable  | enum    |

product_id is the primary key (column with unique values) for this table.\
low_fats is an ENUM (category) of type ('Y', 'N') where 'Y' means this product is low fat and 'N' means it is not.\
Recyclable is an ENUM (category) of types ('Y', 'N') where 'Y' means this product is recyclable and 'N' means it is not.
 

Write a solution to find the ids of products that are both low fat and recyclable.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: \
Products table:

| product_id  | low_fats | recyclable |
|-------------|----------|------------|
| 0           | Y        | N          |
| 1           | Y        | Y          |
| 2           | N        | Y          |
| 3           | Y        | Y          |
| 4           | N        | N          |

Output: 

| product_id  |
|-------------|
| 1           |
| 3           |

Explanation: Only products 1 and 3 are both low fat and recyclable.


### SQL Schema
Create table If Not Exists Products (product_id int, low_fats ENUM('Y', 'N'), recyclable ENUM('Y','N'))\
Truncate table Products\
insert into Products (product_id, low_fats, recyclable) values ('0', 'Y', 'N')\
insert into Products (product_id, low_fats, recyclable) values ('1', 'Y', 'Y')\
insert into Products (product_id, low_fats, recyclable) values ('2', 'N', 'Y')\
insert into Products (product_id, low_fats, recyclable) values ('3', 'Y', 'Y')\
insert into Products (product_id, low_fats, recyclable) values ('4', 'N', 'N')

### What's the trick? (Spoiler)

Simple filter question.

### Solution

```sql

-- Solution

SELECT
    product_id
FROM products
WHERE low_fats = 'Y'
AND recyclable = 'Y';
