### 1. Each company in the accounts table wants to create an email address for each primary_poc. The email address should be the first name of the primary_poc . last name primary_poc @ company name .com.

	WITH t1 AS (SELECT LOWER(name) as name, primary_poc, LEFT(primary_poc, POSITION(' ' IN primary_poc)-1) AS left_name, RIGHT(primary_poc, LENGTH(primary_poc)- POSITION(' ' IN primary_poc)) as right_name

    FROM accounts)

	SELECT name, primary_poc, CONCAT(left_name, '.', right_name, '@', name, '.com') AS email

	FROM t1

### 2. You may have noticed that in the previous solution some of the company names include spaces, which will certainly not work in an email address. See if you can create an email address that will work by removing all of the spaces in the account name, but otherwise your solution should be just as in question 1.

	WITH t1 AS (SELECT REPLACE(name, ' ','') as name, primary_poc, LEFT(primary_poc, POSITION(' ' IN primary_poc)-1) AS left_name, RIGHT(primary_poc, LENGTH(primary_poc)- POSITION(' ' IN primary_poc)) as right_name

    	FROM accounts)

SELECT name, primary_poc, CONCAT(left_name, '.', right_name, '@', name, '.com') AS email

FROM t1

### 3. We would also like to create an initial password, which they will change after their first log in. The first password will be the first letter of the primary_poc's first name (lowercase), then the last letter of

their first name (lowercase), the first letter of their last name (lowercase), the last letter of their last name (lowercase), the number of letters in their first name, the number of letters in their last name, and

then the name of the company they are working with, all capitalized with no spaces.

	WITH t1 AS (SELECT REPLACE(name, ' ','') as name, primary_poc, LEFT(primary_poc, POSITION(' ' IN primary_poc)-1) AS left_name, RIGHT(primary_poc,

		LENGTH(primary_poc) - POSITION(' ' IN primary_poc)) as right_name

    	FROM accounts),

	t2 AS (	SELECT primary_poc, LOWER(LEFT(left_name, 1)) as left1,

      	LOWER(RIGHT(left_name, 1)) leftl, LOWER(LEFT(right_name, 1)) as right1,

      	LOWER(RIGHT(right_name, 1)) rightl, LENGTH(left_name) len,

      	UPPER(name) comp

		FROM t1 )                                                        

	SELECT primary_poc, CONCAT(left1, leftl, right1, rightl, len, comp) AS password

            FROM t2  