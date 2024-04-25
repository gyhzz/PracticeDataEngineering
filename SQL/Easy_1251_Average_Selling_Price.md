# 1251. Average Selling Price (Easy)

Link to question: https://leetcode.com/problems/average-selling-price/description/?envType=study-plan-v2&envId=top-sql-50

Difficulty: Easy

### Description

Table: Prices


| Column Name   | Type    |
|---------------|---------|
| product_id    | int     |
| start_date    | date    |
| end_date      | date    |
| price         | int     |

(product_id, start_date, end_date) is the primary key (combination of columns with unique values) for this table.\
Each row of this table indicates the price of the product_id in the period from start_date to end_date.\
For each product_id there will be no two overlapping periods. That means there will be no two intersecting periods for the same product_id.
 

Table: UnitsSold


| Column Name   | Type    |
|---------------|---------|
| product_id    | int     |
| purchase_date | date    |
| units         | int     |

This table may contain duplicate rows.\
Each row of this table indicates the date, units, and product_id of each product sold. 
 

Write a solution to find the average selling price for each product. average_price should be rounded to 2 decimal places.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input:\
Prices table:

| product_id | start_date | end_date   | price  |
|------------|------------|------------|--------|
| 1          | 2019-02-17 | 2019-02-28 | 5      |
| 1          | 2019-03-01 | 2019-03-22 | 20     |
| 2          | 2019-02-01 | 2019-02-20 | 15     |
| 2          | 2019-02-21 | 2019-03-31 | 30     |

UnitsSold table:

| product_id | purchase_date | units |
|------------|---------------|-------|
| 1          | 2019-02-25    | 100   |
| 1          | 2019-03-01    | 15    |
| 2          | 2019-02-10    | 200   |
| 2          | 2019-03-22    | 30    |

Output: 

| product_id | average_price |
|------------|---------------|
| 1          | 6.96          |
| 2          | 16.96         |

Explanation:\
Average selling price = Total Price of Product / Number of products sold.\
Average selling price for product 1 = ((100 * 5) | (15 * 20)) / 115 = 6.96\
Average selling price for product 2 = ((200 * 15) | (30 * 30)) / 230 = 16.96



### SQL Schema
Create table If Not Exists Weather (id int, recordDate date, temperature int)\
Truncate table Weather\
insert into Weather (id, recordDate, temperature) values ('1', '2015-01-01', '10')\
insert into Weather (id, recordDate, temperature) values ('2', '2015-01-02', '25')\
insert into Weather (id, recordDate, temperature) values ('3', '2015-01-03', '20')\
insert into Weather (id, recordDate, temperature) values ('4', '2015-01-04', '30')

### What's the trick? (Spoiler)

First of all, you need to associate the purchase dates with the price start and end dates and you can do this with an extra join condition of AND u.purchase_date BETWEEN p.start_date AND p.end_date. This ensures that you match the right units count with the price. You also need to use a LEFT JOIN instead of an INNER JOIN since there could be products that have not been sold and hence, will not have a record in the UnitsSold table. Their average selling price should be zero.

The rest is pretty straightforward. To calculate average price, you need to the total revenue from all units sold, regardless of price, and divide it with the total number of units sold. SUM(u.units * p.price) will give you the total revenue of a product sold at different prices, and SUM(u.units) will give you the total units sold. Round this value to 2 decimal places.

For products that have not sold any units, their SUM(u.units) value will be 0 and dividing by zero will produce a null value. You can use COALESCE or IFNULL functions with a 0 to replace that value with 0.

### Solution

```sql

-- Solution 1:

SELECT
    p.product_id,
    COALESCE(ROUND(SUM(u.units * p.price) / SUM(u.units), 2), 0) AS average_price
FROM prices p
LEFT JOIN unitssold u
ON p.product_id = u.product_id
AND purchase_date BETWEEN start_date AND end_date
GROUP BY p.product_id;
