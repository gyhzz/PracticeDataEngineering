# 1164. Product Price at a Given Date (Medium)

Link to question: https://leetcode.com/problems/product-price-at-a-given-date/?envType=study-plan-v2&envId=top-sql-50

Difficulty: Medium

### Description

Table: Products

| Column Name  | Type |
|--------------|------|
| product_id   | int  |
| new_price    | int  |
| change_date  | date |

(product_id, change_date) is the primary key (combination of columns with unique values) of this table.\
Each row of this table indicates that the price of some product was changed to a new price at some date.
 

Write a solution to find the prices of all products on 2019-08-16. Assume the price of all products before any change is 10.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input:\
Products table:

| product_id | new_price | change_date |
|------------|-----------|-------------|
| 1          | 20        | 2019-08-14  |
| 2          | 50        | 2019-08-14  |
| 1          | 30        | 2019-08-15  |
| 1          | 35        | 2019-08-16  |
| 2          | 65        | 2019-08-17  |
| 3          | 20        | 2019-08-18  |

Output: 

| product_id | price |
|------------|-------|
| 2          | 50    |
| 1          | 35    |
| 3          | 10    |

### SQL Schema
Create table If Not Exists Products (product_id int, new_price int, change_date date)\
Truncate table Products\
insert into Products (product_id, new_price, change_date) values ('1', '20', '2019-08-14')\
insert into Products (product_id, new_price, change_date) values ('2', '50', '2019-08-14')\
insert into Products (product_id, new_price, change_date) values ('1', '30', '2019-08-15')\
insert into Products (product_id, new_price, change_date) values ('1', '35', '2019-08-16')\
insert into Products (product_id, new_price, change_date) values ('2', '65', '2019-08-17')\
insert into Products (product_id, new_price, change_date) values ('3', '20', '2019-08-18')

### Solution

```sql

-- Solution 1

WITH cte AS (
    SELECT
        product_id,
        MAX(change_date) AS last_date
    FROM products
    WHERE change_date <= '2019-08-16'
    GROUP BY product_id
)

, changed_prices AS (
    SELECT
        product_id,
        new_price AS price
    FROM products
    WHERE (product_id, change_date) IN (SELECT product_id, last_date FROM cte)
)

SELECT * FROM changed_prices
UNION
SELECT product_id, 10 AS price
FROM products
WHERE product_id NOT IN (SELECT product_id FROM changed_prices);

-- Solution 2

WITH cte AS (
    SELECT
        product_id,
        new_price,
        change_date,
        ROW_NUMBER() OVER(PARTITION BY product_id ORDER BY change_date DESC) AS rnk
    FROM products
    WHERE change_date <= '2019-08-16'
)

SELECT
    p.product_id AS product_id,
    10 AS price
FROM products p
WHERE product_id NOT IN (SELECT product_id FROM cte)
UNION
SELECT 
    product_id, 
    new_price
FROM cte
WHERE rnk = 1;
