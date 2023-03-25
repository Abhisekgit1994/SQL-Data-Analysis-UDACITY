### 1. Provide the name of the sales_rep in each region with the largest amount of total_amt_usd sales.

WITH t1 AS (SELECT r.name region, s.name sales_name, SUM(o.total_amt_usd) total_sum
from region r
JOIN sales_reps s
ON r.id = s.region_id
JOIN accounts a
ON s.id = a.sales_rep_id
JOIN orders o
ON a.id = o.account_id
GROUP BY 1,2
ORDER BY 3 DESC),

t2 AS (SELECT region, MAX(total_sum) max_sales
         FROM t1
         GROUP BY 1 )
SELECT t1.sales_name,t1.region, t1.total_sum
FROM t1
JOIN t2
ON t1.region=t2.region and t1.total_sum = t2.max_sales

### 2. For the region with the largest sales total_amt_usd, how many total orders were placed?

WITH t1 AS (SELECT r.name region, SUM(o.total_amt_usd) total_sum
from region r
JOIN sales_reps s
ON r.id = s.region_id
JOIN accounts a
ON s.id = a.sales_rep_id
JOIN orders o
ON a.id = o.account_id
           GROUP BY r.name),

t2 AS (
      SELECT MAX(total_sum)
      FROM t1)

SELECT r.name, COUNT(o.total) total_orders
FROM sales_reps s
JOIN accounts a
ON a.sales_rep_id = s.id
JOIN orders o
ON o.account_id = a.id
JOIN region r
ON r.id = s.region_id
GROUP BY r.name
HAVING SUM(o.total_amt_usd) = (SELECT * FROM t2);



### 3. How many accounts had more total purchases than the account name which has bought the most standard_qty paper throughout their lifetime as a customer?

WITH t1 AS(SELECT a.name acc_name, SUM(o.standard_qty) st_qty, SUM(o.total) total
FROM accounts a
JOIN orders o
ON a.id = o.account_id
GROUP BY 1
ORDER BY 2 DESC
          LIMIT 1),
t2 AS (select total from t1),

t3 AS (SELECT a.name a_name, SUM(o.total) total_sum
FROM accounts a
JOIN orders o
ON a.id = o.account_id
GROUP BY 1
HAVING SUM(o.total) > (SELECT * FROM t2))
SELECT COUNT(*) FROM t3