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
```
--by city
SELECT 
    alls.city,
	alls.country, 
    AVG(sr.total_ordered)
FROM sales_report AS sr
JOIN all_sessions AS alls
ON sr.productsku = alls.productsku
WHERE
    city IS NOT NULL 
    AND city != 'N/A'
GROUP BY alls.city, alls.country
ORDER BY avg DESC
LIMIT 15
```

Answer:I adjusted the query to filter results for the top 25 for country and 15 for city, for the sake of organization.<br>

By Country:<br>
"Saudi Arabia" =	134.80<br>
"Kuwait" =	114.33<br>
"Honduras" =	112.00<br>
"Ethiopia" =	85.00<br>
"Laos" =	85.00<br>
"Oman" =	85.00<br>
"United Arab Emirates" =	65.13<br>
"Papua New Guinea" =	57.50<br>
"Croatia" =	57.44<br>
"South Korea" =	56.08<br>
"San Marino" =	50.00<br>
"Albania" =	50.00<br>
"Greece" =	47.55<br>
"Trinidad & Tobago" =	47.00<br>
"Hong Kong" =	44.88<br>
"Kazakhstan" =	43.00<br>
"Czechia" =	39.48<br>
"Italy" =	38.63<br>
"Guatemala" =	37.86<br>
"Denmark" =	37.56<br>
"Taiwan" =	36.31<br>
"Israel" =	35.88<br>
"Japan" =	35.60<br>
"Spain" =	35.30<br>
"Mexico" =	35.13<br>

By City:
"Riyadh"	"Saudi Arabia"	319.00
"Brno"	"Czechia"	319.00
"Rexburg"	"United States"	250.50
"Sacramento"	"United States"	189.00
"Lisbon"	"Portugal"	189.00
"Kalamazoo"	"United States"	105.00
"Saint Petersburg"	"Russia"	101.25
"Avon"	"United States"	100.00
"Rome"	"Italy"	97.75
"Longtan District"	"Taiwan"	97.00
"Sherbrooke"	"Canada"	97.00
"Nashville"	"United States"	94.00
"Dubai"	"United Arab Emirates"	89.20
"Shinjuku"	"Japan"	73.00
"Pune"	"India"	71.08


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer: This question is vague, so I'm taking "impact" to mean  







