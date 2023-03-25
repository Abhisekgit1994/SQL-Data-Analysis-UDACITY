### 1. In the accounts table, there is a column holding the website for each company. The last three digits specify what type of web address they are using. A list of extensions (and pricing) is provided here. Pull these extensions and provide how many of each website type exist in the accounts table.

SELECT RIGHT(website, 3) AS domain, COUNT(*) num_companies
FROM accounts
GROUP BY 1
ORDER BY 2 DESC;


### 2. There is much debate about how much the name (or even the first letter of a company name) matters. Use the accounts table to pull the first letter of each company name to see the distribution of company names that begin with each letter (or number).

SELECT LEFT(UPPER(name), 1) AS first_letter, COUNT(*) num_companies
FROM accounts
GROUP BY 1
ORDER BY 2 DESC;


### 3. Consider vowels as a, e, i, o, and u. What proportion of company names start with a vowel, and what percent start with anything else?

WITH t1 AS (select name, CASE WHEN LEFT(name, 1) IN ('A','E', 'I', 'O', 'U',
   'a', 'e', 'i', 'o', 'u') THEN 'vowels' ELSE 'others' END AS Vowels
FROM accounts),
t2 AS (SELECT COUNT(*) coun from accounts),
t3 AS (SELECT vowels, COUNT(vowels) total
FROM t1
GROUP BY vowels)
SELECT vowels, round((total*100/351), 2) prop
FROM t3



