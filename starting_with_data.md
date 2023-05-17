Question 1: What one item has been ordered the most?

SQL Queries:
SELECT alls.fullvisitorid, sr.productsku, SUM(alls.productprice * sr.total_ordered) AS amount_spent
FROM all_sessions AS alls
JOIN sales_report AS sr
ON sr.productsku = alls.productsku
WHERE LENGTH(alls.fullvisitorid) = 19 AND total_ordered != 0
GROUP BY alls.fullvisitorid, sr.productsku
ORDER BY amount_spent DESC
LIMIT 100

Answer: 



Question 2: 

SQL Queries:

Answer:



Question 3: 

SQL Queries:

Answer:
