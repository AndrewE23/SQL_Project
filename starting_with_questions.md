Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:<br>
```
--By country
SELECT 
 country, 
 SUM(totaltransactionrevenue) AS transaction_by_country
FROM all_SESSIONS
WHERE transactions IS NOT NULL AND transactions > 0 AND country != 'N/A'
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
WHERE country != 'N/A'
GROUP BY country
ORDER BY avg DESC
LIMIT 25
```
```
--by city
SELECT 
    alls.city ||', '|| alls.country AS location_avg, 
    AVG(sr.total_ordered)
FROM sales_report AS sr
JOIN all_sessions AS alls
ON sr.productsku = alls.productsku
WHERE
    city != 'N/A'
GROUP BY alls.city, alls.country
ORDER BY avg DESC
LIMIT 15
```

Answer: I filtered results for the top 25 for country and 15 for city, for the sake of organization.<br>

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

By City:<br>
"Riyadh, Saudi Arabia" = 	319.00<br>
"Brno, Czechia" =	319.00<br>
"Rexburg, United States" =	250.50<br>
"Sacramento, United States" =	189.00<br>
"Lisbon, Portugal" =	189.00<br>
"Kalamazoo, United States" =	105.00<br>
"Saint Petersburg, Russia" =	101.25<br>
"Avon, United States" =	100.00<br>
"Rome, Italy" =	97.75<br>
"Longtan District, Taiwan" =	97.00<br>
"Sherbrooke, Canada" =	97.00<br>
"Nashville, United States" =	94.00<br>
"Dubai, United Arab Emirates" =	89.20<br>
"Shinjuku, Japan" =	73.00<br>
"Pune, India" =	71.08<br>


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**

SQL Queries:
```
--by country
SELECT 
	alls.country, 
	alls.v2productname AS product_name, 
	TRIM(LEADING 'Home/' FROM v2productcategory), 
	SUM(sr.total_ordered) AS order_set
FROM all_sessions AS alls
JOIN sales_report AS sr
ON alls.v2productname = sr.name
WHERE country != 'N/A' AND v2productcategory != '(not set)'
GROUP BY alls.country, alls.v2productcategory, alls.v2productname
ORDER BY alls.country, order_set DESC
LIMIT 10
```
```
--by city
SELECT 
	alls.city || ', ' || alls.country AS location, 
	alls.v2productname AS product_name, 
	TRIM(LEADING 'Home/' FROM v2productcategory), 
	SUM(sr.total_ordered) AS order_set
FROM all_sessions AS alls
JOIN sales_report AS sr
ON alls.v2productname = sr.name
WHERE country  != 'N/A' AND v2productcategory != '(not set)' AND city != 'N/A'
GROUP BY alls.city, alls.country, alls.v2productcategory, alls.v2productname
ORDER BY alls.country, order_set DESC
LIMIT 10
```
Answer: One constraint of method is that it does not fully clean the v2productcategory values of url snippets, but that dataset is inconsistent anyhow so at least trimming any instances of "Home/" allows people to study the results and see which categories people shop for the most by country.

This method gives us a useful insight; for example, products classed as lifestyle or accessory items are most frequently ordered from the site in Argentina. 


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```
--by country
SELECT DISTINCT ON (country)
	alls.country, 
	alls.v2productname AS product_name, 
	SUM(sr.total_ordered) AS most_sold
FROM all_sessions AS alls
JOIN sales_report AS sr
ON alls.v2productname = sr.name
WHERE country != 'N/A'
GROUP BY alls.country, alls.v2productname
ORDER BY alls.country, most_sold DESC
```
```
--by city
SELECT DISTINCT ON (location)
	alls.city || ', ' || alls.country AS location, 
	alls.v2productname AS product_name, 
	SUM(sr.total_ordered) AS most_sold
FROM all_sessions AS alls
JOIN sales_report AS sr
ON alls.v2productname = sr.name
WHERE country != 'N/A' AND city != 'N/A'
GROUP BY location, alls.v2productname
ORDER BY location, most_sold DESC
```

Answer: This query returns only the first result based on the defined column, which in this case is "most_sold", for each country/city. With this data, we can determine which products do better in which locations, which can be useful from a marketing perspective.


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
```
--Set up a CTE
WITH regionalorders AS (
  SELECT country, total_ordered, SUM(total_ordered) AS no_ordered
  FROM all_sessions AS alls
  JOIN sales_report AS sr 
  ON alls.productsku = sr.productsku
  WHERE country != 'N/A'
  GROUP BY country, total_ordered
)
--Grab and join data from CTE 
SELECT alls.country, sales_report.total_ordered, 
       100.0 * sales_report.total_ordered / SUM(sales_report.total_ordered) OVER () AS PercentageOfTotalSales
FROM sales_report
JOIN regionalorders 
ON sales_report.total_ordered = regionalorders.total_ordered
JOIN all_sessions AS alls
ON alls.productsku = sales_report.productsku
GROUP BY alls.country, sales_report.total_ordered
ORDER BY percentageoftotalsales DESC
LIMIT 20
```


Answer: This question is vague, so I'm taking it to mean "which countries have purchased the highest percentage of products total?"







