### 1. Use the accounts table to create first and last name columns that hold the first and last names for the primary_poc.

Select primary_poc, 
LEFT(primary_poc, STRPOS(primary_poc, ' ')-1) left_name, 
RIGHT(primary_poc, LENGTH(primary_poc)-STRPOS(primary_poc, ' ') ) AS right
FROM accounts

### 2.Now see if you can do the same thing for every rep name in the sales_reps table. Again provide first and last name columns.

SELECT name, LEFT(name, POSITION(' ' IN name)-1) AS left,
    RIGHT(name, LENGTH(name) - POSITION(' ' IN name)) AS right
                                
FROM sales_reps