##What are your risk areas? Identify and describe them.##
While I feel like I understand the data fairly well, it is a messy dataset with numerous small issues, a lot of bloat, and plenty of missing data that makes it difficult to fill in the gaps. As such, I identified risk factors based on the data I did have available:<br>
1. There were many NULL columns, as well as some columns where only a small assortment of rows had any distinct values in them whatsoever. The solution was to delete the columns, but I needed to confirm that solution first.<br>
2. I wanted to verify that certain primary keys, as well as certain other keys, were formatted consistently and free of duplicates (wherever it was possible for me to manage that).<br>
3. There were 8 unreferenced entries entirely distinct to *sales_by_sku*, and at the time I was unsure if there were any other inconsistencies between it and the other tables.<br>
4. Given the size of the data tables and the fact it involves business statistics, I also wanted to verify that all dates were within an acceptable, consistent range.<br>

##QA Process:##
Describe your QA process and include the SQL queries used to execute it.

### 1. NULL & Redundancy Checking
First, I wanted to see if any columns were worth nuking.

**all_sessions**
I initially checked to see what these columns contained:<br>
```
--Counting how many entries each column has; none surpass 40 records, out of over 15,000 total.
SELECT COUNT(*) FROM all_sessions
--WHERE searchkeyword IS NOT NULL 
--WHERE productrefundamount IS NOT NULL
--WHERE itemquantity IS NOT NULL
--WHERE itemrevenue IS NOT NULL
--WHERE transactionrevenue IS NOT NULL
--WHERE productrevenue IS NOT NULL
--WHERE transactionid IS NOT NULL
--WHERE searchkeyword IS NOT NULL
--WHERE productvariant IS NOT NULL
```
The syntax provided here is because I checked each column individually, but wanted to keep record of relevant queries while conserving page space; un-comment one line at a time to replicate my process. I ran the same query for every column in every table, and only found reason to delete two columns in *analytics*. The reasoning for this is explained in cleaning_data.md.

I also wanted to check the "currencycode" column:<br>
```
--I want the total number of entries (15134), the total number of currencycodes (14,862), and the number if distinct currencycodes (only one = 'USD')
SELECT COUNT(*) AS totals
FROM all_sessions
UNION
SELECT COUNT (DISTINCT currencycode)
FROM all_sessions
UNION
SELECT COUNT(currencycode)
FROM all_sessions
```
There are only 272 entries that are NULL, and comparing the "productprice" field in several of these results with other entries allowed me to carry on with the assumption that all items are being sold in USD.


### 2. Key/Value Formatting
The main keys I was concerned with were the "fullvisitorid" value, since that variable could be important for identifying unique records, and the various iterations of "productname" for similar reasons:
```
- Return fullvisitorid char length, plus the number of IDs of each length
SELECT 
  COUNT(fullvisitorid) AS total,
  LENGTH(fullvisitorid) AS char_length
FROM all_sessions
GROUP BY LENGTH(fullvisitorid)
```
This confirmed that IDs are meant to be 19 characters long, but I decided to keep the records in the original dataset in part because just over 10% of them are entered incorrectly, but the "visitid" variable is the actual primary key in this table because this table is meant to document total unique sessions, not total unique visitors; one visitor could start a new session and still retain their own personal ID. 

I also noticed some weird spacing issues in the productname fields in *all_sessions*, *products*, and *sales_report* tables:
```
--Check for leading, extra spaces - sales_report
SELECT COUNT(name)
FROM sales_report
WHERE name ILIKE '%  %'
OR name ILIKE ' %'
```
```
--Check for leading, extra spaces - products
SELECT COUNT(productnamename)
FROM products
WHERE name ILIKE '%  %'
OR name ILIKE ' %'
```
```
--Check for leading, extra spaces - all_sessions
SELECT COUNT(v2productname)
FROM all_sessions
WHERE name ILIKE '%  %'
OR name ILIKE ' %'
```

### 3. sales_by_sku
The two columns in *sales_by_sku* appeared to be largely identical to two columns in *sales_report*, so I set up a query to identify missing/unmatching sku numbers as booleans:
```
SELECT sbs.*, sr.productsku IS NOT NULL AS matching_sku
FROM sales_by_sku AS sbs
LEFT JOIN sales_report AS sr
ON sbs.productsku = sr.productsku
ORDER BY matching_sku;
```
This returned 8 false results. However, since key variable "productsku" was also present in the *products* and *all_sessions* tables, I decided to set up a query to check for any keys in this table that were not present in at least one other table. I did this by joining all four tables together:<br>
```
SELECT
  sales_by_sku.*,
  EXISTS(
  	SELECT 1 FROM all_sessions
  	JOIN sales_report
  	ON sales_by_sku.productsku = sales_report.productsku
  	JOIN products
  	ON sales_by_sku.productsku = products.sku
  	WHERE sales_by_sku.productsku = all_sessions.productsku
  	OR sales_by_sku.productsku = sales_report.productsku
  	OR sales_by_sku.productsku = products.sku
  ) AS matching_sku 
FROM sales_by_sku
ORDER BY matching_sku;
```
The same 8 records I noticed before were returned by this query. To be completely safe, I also checked to make sure there were no mismatches in the "total_ordered" columns in both *sales_by_sku* and *sales_report*:
```
SELECT sbs.*, sr.name, sr.total_ordered IS NOT NULL AS matching_sku
FROM sales_by_sku AS sbs
LEFT JOIN sales_report AS sr
ON sbs.productsku = sr.productsku
ORDER BY matching_sku;
```
This confirmed that there were no mismatches in the data, save for those 8 records that are stored only in *sales_by_sku*. I then deleted the table for being a waste of space:

### 4. Date Verification
```
-- Returns MIN date (2016-08-01) and MAX date (2017-08-01)
SELECT MIN(date), MAX(date)
FROM all_sessions
```
```
-- Returns MIN date (2017-05-01) and MAX date (2017-08-01)
SELECT MIN(date), MAX(date)
FROM analytics
```
Both tables have dates that fall within acceptable ranged, and in both table's rows the data is already defined as a date so 
