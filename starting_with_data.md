Question 1: What one item has been ordered the most?

SQL Queries:
```
--Return productsku as a key identifier
SELECT sr.productsku, alls.v2productname, SUM(alls.productprice * sr.total_ordered) AS amount_spent
FROM all_sessions AS alls
JOIN sales_report AS sr
ON sr.productsku = alls.productsku
WHERE LENGTH(alls.fullvisitorid) = 19 AND total_ordered != 0
GROUP BY sr.productsku, alls.v2productname
ORDER BY amount_spent DESC
LIMIT 10
```

Answer: 
>"Nest® Cam Outdoor Security Camera - USA"	1238496
"Nest® Learning Thermostat 3rd Gen-USA - Stainless Steel"	1225948
"Nest® Cam Indoor Security Camera - USA"	1204926
"Google 17oz Stainless Steel Sport Bottle"	336004
"Nest® Protect Smoke + CO White Wired Alarm-USA"	243684
"YouTube Hard Cover Journal"	198900
"Android 17oz Stainless Steel Sport Bottle"	151636
"Nest® Protect Smoke + CO White Battery Alarm-USA"	136392
"Leatherette Journal"	106865
"26 oz Double Wall Insulated Bottle"	97000


Question 2: 

SQL Queries:

Answer:



Question 3: 

SQL Queries:

Answer:
