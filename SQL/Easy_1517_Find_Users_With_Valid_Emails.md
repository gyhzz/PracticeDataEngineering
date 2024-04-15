# 1517. Find Users with Valid E-mails (Easy)

Link to question: https://leetcode.com/problems/find-users-with-valid-e-mails/description/

Difficulty: Easy

### Description

Table: Users


| Column Name   | Type    |
|---------------|---------|
| user_id       | int     |
| name          | varchar |
| mail          | varchar |

user_id is the primary key (column with unique values) for this table.\
This table contains information of the users signed up in a website. Some e-mails are invalid.
 

Write a solution to find the users who have valid emails.

A valid e-mail has a prefix name and a domain where:

The prefix name is a string that may contain letters (upper or lower case), digits, underscore '_', period '.', and/or dash '-'. The prefix name must start with a letter.\
The domain is '@leetcode.com'.\
Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: \
Users table:

| user_id | name      | mail                    |
|---------|-----------|-------------------------|
| 1       | Winston   | winston@leetcode.com    |
| 2       | Jonathan  | jonathanisgreat         |
| 3       | Annabelle | bella-@leetcode.com     |
| 4       | Sally     | sally.come@leetcode.com |
| 5       | Marwan    | quarz#2020@leetcode.com |
| 6       | David     | david69@gmail.com       |
| 7       | Shapiro   | .shapo@leetcode.com     |

Output: 

| user_id | name      | mail                    |
|---------|-----------|-------------------------|
| 1       | Winston   | winston@leetcode.com    |
| 3       | Annabelle | bella-@leetcode.com     |
| 4       | Sally     | sally.come@leetcode.com |

Explanation: \
The mail of user 2 does not have a domain.\
The mail of user 5 has the # sign which is not allowed.\
The mail of user 6 does not have the leetcode domain.\
The mail of user 7 starts with a period.


### SQL Schema
Create table If Not Exists Users (user_id int, name varchar(30), mail varchar(50))\
Truncate table Users\
insert into Users (user_id, name, mail) values ('1', 'Winston', 'winston@leetcode.com')\
insert into Users (user_id, name, mail) values ('2', 'Jonathan', 'jonathanisgreat')\
insert into Users (user_id, name, mail) values ('3', 'Annabelle', 'bella-@leetcode.com')\
insert into Users (user_id, name, mail) values ('4', 'Sally', 'sally.come@leetcode.com')\
insert into Users (user_id, name, mail) values ('5', 'Marwan', 'quarz#2020@leetcode.com')\
insert into Users (user_id, name, mail) values ('6', 'David', 'david69@gmail.com')\
insert into Users (user_id, name, mail) values ('7', 'Shapiro', '.shapo@leetcode.com')

### What's the trick? (Spoiler)

Straight-forward regex string matching question. 

^[A-Za-z]: Check that the first character of the email address is a letter

[A-Za-z0-9_.-]*: Check that the email address only contain letters, numbers, and the 3 allowed characters '_', '.', '-', zero or more times

@leetcode[.]com$: Check that the domain of the email address is @leetcode.com. The [.] is to match an actual period instead of the regex meaning of . (match any single character). One of the edge cases will have a domain of @leetcode?com and this will cover it. Also check that this is the last part of the string and nothing comes after this.

### Solution

```sql

-- Solution

SELECT
    user_id,
    name,
    mail
FROM users
WHERE mail REGEXP '^[A-Za-z][A-Za-z0-9_.-]*@leetcode[.]com$';
