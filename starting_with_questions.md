Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:<br>
By Country:
```
SELECT 
 country, 
 SUM(totaltransactionrevenue) AS transaction_by_country
FROM all_SESSIONS
WHERE transactions IS NOT NULL AND transactions > 0
GROUP BY country 
ORDER BY transaction_by_country DESC
```
By City:
```
SELECT 
 city, 
 SUM(totaltransactionrevenue) AS transaction_by_city
FROM all_SESSIONS
WHERE transactions IS NOT NULL AND transactions > 0 AND city
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



Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







