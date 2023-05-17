**Question 1: Which items have been purchased the most, sorted by total price for all items?**

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
"Nest® Cam Outdoor Security Camera - USA" = 	1,238,496<br>
"Nest® Learning Thermostat 3rd Gen-USA - Stainless Steel" =	1,225,948<br>
"Nest® Cam Indoor Security Camera - USA" =	1,204,926<br>
"Google 17oz Stainless Steel Sport Bottle" =	336,004<br>
"Nest® Protect Smoke + CO White Wired Alarm-USA" =	243,684<br>
"YouTube Hard Cover Journal" =	198,900<br>
"Android 17oz Stainless Steel Sport Bottle" =	151,636<br>
"Nest® Protect Smoke + CO White Battery Alarm-USA" =	136,392<br>
"Leatherette Journal" =	106,865<br>
"26 oz Double Wall Insulated Bottle" =	97,000<br>


**Question 2: What are the total_orders for each category of channelgrouping? What do the results suggest about how effective one avenue of access to the site is for selling products to people?**

SQL Queries:
```
SELECT 
  DISTINCT alls.channelgrouping, 
  COUNT(alls.channelgrouping), 
  SUM(sr.total_ordered) AS total_orders
FROM all_sessions AS alls
JOIN sales_report AS sr
ON alls.productsku = sr.productsku
WHERE total_ordered != 0
GROUP BY channelgrouping
```

Answer: The top pulls to the website by far are organic searches, followed by people who had bookmarked the site or a page on it prior to purchasing, and then referrals. This suggests that people only come to the site when they are actively looking for something, and that other channel groupings are not nearly as effective at drawing in new customers.

**"(Other)"**<br>
Count: 4<br>
total_orders: 48<br>

**"Affiliates"**<br>
Count: 140<br>
total_orders: 4806<br>

**"Direct"**<br>
Count: 1493<br>
total_orders: 44154<br>

**"Display"**<br>
Count: 70<br>
total_orders: 2369<br>

**"Organic Search"**<br>	
Count: 4216<br>
total_orders: 104747<br>

**"Paid Search"**<br>
Count: 274<br>
total_orders: 6628<br>

**"Referral"**<br>	
Count: 1414<br>
total_orders: 47851<br>



**Question 3: How many orders in each category were made in Q2 of 2017 (from May 1st to July 31st)?**
```
SQL Queries:
--focuses results on Q2 of 2017
SELECT TRIM(LEADING 'Home/' FROM alls.v2productcategory), 
       SUM(sr.total_ordered) AS order_total
  FROM all_sessions AS alls
  JOIN sales_report AS sr
  ON alls.productsku = sr.productsku
  WHERE alls.v2productcategory != '(not set)' AND sr.total_ordered != 0 AND
  alls.date BETWEEN '2017-05-01' and '2017-07-31'
GROUP BY alls.v2productcategory
ORDER BY order_total DESC
```
Answer: Limiting it to the top 10 results for each category, we have:<br>
"Shop by Brand/YouTube/" = 	14787<br>
"Nest/Nest-USA/" =	7311<br>
"Drinkware/" =	6103<br>
"Office/Notebooks & Journals/" =	2897<br>
"Bags/" =	2888<br>
"Apparel/" =	2521<br>
"Electronics/" =	2516<br>
"Drinkware/Water Bottles and Tumblers/" =	2157<br>
"Shop by Brand/Google/" =	1703<br>
"Office/" =	1361<br>
