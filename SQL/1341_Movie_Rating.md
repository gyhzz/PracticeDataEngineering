# 1341. Movie Rating

Link to question: https://leetcode.com/problems/movie-rating/description/?envType=study-plan-v2&envId=top-sql-50

### Description

Table: Movies

| Column Name   | Type    |
|---------------|---------|
| movie_id      | int     |
| title         | varchar |

movie_id is the primary key (column with unique values) for this table.\
title is the name of the movie.
 

Table: Users


| Column Name   | Type    |
|---------------|---------|
| user_id       | int     |
| name          | varchar |

user_id is the primary key (column with unique values) for this table.
 

Table: MovieRating


| Column Name   | Type    |
|---------------|---------|
| movie_id      | int     |
| user_id       | int     |
| rating        | int     |
| created_at    | date    |

(movie_id, user_id) is the primary key (column with unique values) for this table.\
This table contains the rating of a movie by a user in their review.\
created_at is the user's review date. 
 

Write a solution to:

Find the name of the user who has rated the greatest number of movies. In case of a tie, return the lexicographically smaller user name.\
Find the movie name with the highest average rating in February 2020. In case of a tie, return the lexicographically smaller movie name.\
The result format is in the following example.

 

Example 1:

Input:\
Movies table:

| movie_id    |  title       |
|-------------|--------------|
| 1           | Avengers     |
| 2           | Frozen 2     |
| 3           | Joker        |

Users table:

| user_id     |  name        |
|-------------|--------------|
| 1           | Daniel       |
| 2           | Monica       |
| 3           | Maria        |
| 4           | James        |

MovieRating table:

| movie_id    | user_id      | rating       | created_at  |
|-------------|--------------|--------------|-------------|
| 1           | 1            | 3            | 2020-01-12  |
| 1           | 2            | 4            | 2020-02-11  |
| 1           | 3            | 2            | 2020-02-12  |
| 1           | 4            | 1            | 2020-01-01  |
| 2           | 1            | 5            | 2020-02-17  | 
| 2           | 2            | 2            | 2020-02-01  | 
| 2           | 3            | 2            | 2020-03-01  |
| 3           | 1            | 3            | 2020-02-22  | 
| 3           | 2            | 4            | 2020-02-25  | 

Output: 

| results      |
|--------------|
| Daniel       |
| Frozen 2     |

Explanation:\
Daniel and Monica have rated 3 movies ("Avengers", "Frozen 2" and "Joker") but Daniel is smaller lexicographically.\
Frozen 2 and Joker have a rating average of 3.5 in February but Frozen 2 is smaller lexicographically.

### SQL Schema
Create table If Not Exists Movies (movie_id int, title varchar(30))\
Create table If Not Exists Users (user_id int, name varchar(30))\
Create table If Not Exists MovieRating (movie_id int, user_id int, rating int, created_at date)\
Truncate table Movies\
insert into Movies (movie_id, title) values ('1', 'Avengers')\
insert into Movies (movie_id, title) values ('2', 'Frozen 2')\
insert into Movies (movie_id, title) values ('3', 'Joker')\
Truncate table Users\
insert into Users (user_id, name) values ('1', 'Daniel')\
insert into Users (user_id, name) values ('2', 'Monica')\
insert into Users (user_id, name) values ('3', 'Maria')\
insert into Users (user_id, name) values ('4', 'James')\
Truncate table MovieRating\
insert into MovieRating (movie_id, user_id, rating, created_at) values ('1', '1', '3', '2020-01-12')\
insert into MovieRating (movie_id, user_id, rating, created_at) values ('1', '2', '4', '2020-02-11')\
insert into MovieRating (movie_id, user_id, rating, created_at) values ('1', '3', '2', '2020-02-12')\
insert into MovieRating (movie_id, user_id, rating, created_at) values ('1', '4', '1', '2020-01-01')\
insert into MovieRating (movie_id, user_id, rating, created_at) values ('2', '1', '5', '2020-02-17')\
insert into MovieRating (movie_id, user_id, rating, created_at) values ('2', '2', '2', '2020-02-01')\
insert into MovieRating (movie_id, user_id, rating, created_at) values ('2', '3', '2', '2020-03-01')\
insert into MovieRating (movie_id, user_id, rating, created_at) values ('3', '1', '3', '2020-02-22')\
insert into MovieRating (movie_id, user_id, rating, created_at) values ('3', '2', '4', '2020-02-25')

### Solution

```sql

-- Solution 1: Using GROUP BY, ORDER BY, and LIMIT

SELECT 
    name AS results
FROM
    (SELECT
        u.name
    FROM movierating m
    INNER JOIN users u
    ON m.user_id = u.user_id
    GROUP BY m.user_id
    ORDER BY COUNT(*) DESC, u.name ASC
    LIMIT 1) a
UNION ALL
SELECT 
    title AS results
FROM
    (SELECT
        m.title
    FROM movierating mr
    INNER JOIN movies m
    ON mr.movie_id = m.movie_id
    WHERE YEAR(created_at) = '2020' AND MONTH(created_at) = '02'
    GROUP BY m.title
    ORDER BY AVG(rating) DESC, m.title ASC
    LIMIT 1) b;

-- Solution 2: Using window functions

SELECT
    name AS results
FROM
    (SELECT 
        name,
        ROW_NUMBER() OVER(ORDER BY user_count DESC, name ASC) AS rnk
    FROM
        (SELECT DISTINCT
            u.name,
            COUNT(*) OVER(PARTITION BY mr.user_id) AS user_count
        FROM movierating mr
        INNER JOIN users u
        ON mr.user_id = u.user_id) a) b
WHERE rnk = 1

UNION ALL

SELECT
    title
FROM
    (SELECT
        title,
        ROW_NUMBER() OVER(ORDER BY avg_rating DESC, title ASC) AS rnk
    FROM
        (SELECT DISTINCT
            m.title,
            AVG(rating) OVER(PARTITION BY mr.movie_id) AS avg_rating
        FROM movierating mr
        INNER JOIN movies m
        ON mr.movie_id = m.movie_id
        WHERE YEAR(created_at) = '2020' AND MONTH(created_at) = '02') c) d
WHERE rnk = 1;