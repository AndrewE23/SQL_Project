Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:<br>
```
--By country
SELECT 
 country, 
 SUM(totaltransactionrevenue) AS transaction_by_country
FROM all_SESSIONS
WHERE transactions IS NOT NULL AND transactions > 0
GROUP BY country 
ORDER BY transaction_by_country DESC
```
```
--By city
SELECT 
 city, 
 SUM(totaltransactionrevenue) AS transaction_by_city
FROM all_SESSIONS
WHERE transactions IS NOT NULL AND transactions > 0 AND city != 'N/A'
GROUP BY city 
ORDER BY transaction_by_city DESC
LIMIT 10
```

Answer: Based on what data is available:

By Country:<br>
"United States"<br>
"Israel"<br>
"Australia"<br>
"Canada"<br>
"Switzerland"<br>
No other countries show up on the list.

By City:<br>
"San Francisco"<br>
"Sunnyvale"<br>
"Atlanta"<br>
"Palo Alto"<br>
"Tel Aviv-Yafo"<br>
"New York"<br>
"Mountain View"<br>
"Los Angeles"<br>
"Chicago"<br>
"Seattle"<br>




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
```
--by country
SELECT alls.country, AVG(sr.total_ordered) 
FROM sales_report AS sr
JOIN all_sessions AS alls
ON sr.productsku = alls.productsku
WHERE country IS NOT NULL
GROUP BY country
ORDER BY avg DESC
LIMIT 25
```
```
--by city
SELECT 
	alls.city, 
	AVG(sr.total_ordered)
FROM sales_report AS sr
JOIN all_sessions AS alls
ON sr.productsku = alls.productsku
WHERE 	
	city IS NOT NULL 
	AND city != 'N/A'
GROUP BY alls.city
ORDER BY avg DESC
LIMIT 25
```



Answer:I adjusted the query to filter results for the top 25 for the sake of organization.<br>

By Country:<br>
"Saudi Arabia" =	134.80
"Kuwait" =	114.33
"Honduras" =	112.00
"Ethiopia" =	85.00
"Laos" =	85.00
"Oman" =	85.00
"United Arab Emirates" =	65.13
"Papua New Guinea" =	57.50
"Croatia" =	57.44
"South Korea" =	56.08
"San Marino" =	50.00
"Albania" =	50.00
"Greece" =	47.55
"Trinidad & Tobago" =	47.00
"Hong Kong" =	44.88
"Kazakhstan" =	43.00
"Czechia" =	39.48
"Italy" =	38.63
"Guatemala" =	37.86
"Denmark" =	37.56
"Taiwan" =	36.31
"Israel" =	35.88
"Japan" =	35.60
"Spain" =	35.30
"Mexico" =	35.13



**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







