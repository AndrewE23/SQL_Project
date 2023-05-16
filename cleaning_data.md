# What issues will you address by cleaning the data?
## Issues
What issues will you address by cleaning the data?

### Specific Issues
>#1a: There are many data fields that were largely or entirely empty (i.e. NULL), especially in the *all_sessions* table. Keeping them around would bog down loading and runtime speeds, as well as increase space used, and there would be little point anyway since data that is not available for use can not provide any meaningful insights. There were cases where a tiny fraction of a table's records had a value in these columns, but at that point there was still the issue of scale; I cannot extrapolate meaningful results from 30 values out of a total dataset of 15,000 records.

>#1b. Similarly, there were some columns where there was only one value stored, or else overwhelmingly one value and then a small portion that were NULL.<br> 
i) The "currencycode" column in *all_sessions* was almost entirely filled with 'USD', and if it wasn't then entries simply left it as NULL. As such, my assumption is that everything is being sold in USD and this column was redundant.<br> 
ii) The "socialengagementtype" column in *analytics* is also only ever one value.

>#2. The *sales_by_sku* table, which contained two columns ("productsku" and "total_ordered"), was entirely redundant. I first noticed that they were almost identical to two columns in the *sales_report* table, which prompted me to compare it with the three other tables that include a sku/productsku column. The verdict: All but 8 records had match in at least one other table. Since the table was worthless, I decided it would be better to simply delete it.

>#3. Monetary amounts in various columns were not formatted like currency, needing to be divided by 1,000,000 and rounded to two decimal points to better reflect what they are.

>#4: There is a lot of duplicate data in the *all_sessions* table, which had to be filtered out because duplicate data can skew any results I generate and call into question the quality of the data or my methods for parsing that data. 

>#5: In the *all_sessions* table, country is sometimes maked "(not set)", and city is sometimes either "(not set)" or "not available in demo dataset". Any "(not set)" values can easily be replaced with NULL, while the other city string may have to be 

<hr>

## Queries

Below, provide the SQL queries you used to clean your data.

### Issue #1: Remove Irrelevant Data
Focus: **all_sessions**<br>

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
The syntax provided here is because I checked each column individually, but wanted to keep record of relevant queries; un-comment one line at a time to replicate my process. 

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

```
Column Deletion:<br>
>ALTER TABLE all_sessions
DROP COLUMN searchkeyword,
DROP COLUMN productrefundamount,
DROP COLUMN itemquantity,
DROP COLUMN itemrevenue,
DROP COLUMN transactionrevenue,
DROP COLUMN productrevenue,
DROP COLUMN transactionid,
DROP COLUMN ecommerce_option,
DROP COLUMN productvariant,
DROP COLUMN currencycode;
```

Focus: **all_sessions**<br>
```
ALTER TABLE analytics
DROP COLUMN userid, -- All NULL
DROP COLUMN socialengagementtype; -- Only a single kind of value
```

### Issue #2: sales_by_sku
The two columns in *sales_by_sku* appear to be largely identical to two columns in *sales_report*, so I set up a query to identify missing/unmatching sku numbers as booleans:
```
SELECT sbs.*, sr.productsku IS NOT NULL AS matching_sku
FROM sales_by_sku AS sbs
LEFT JOIN sales_report AS sr
ON sbs.productsku = sr.productsku
ORDER BY matching_sku
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
ORDER BY matching_sku
```
The same 8 records I noticed before were returned by this query. To be completely safe, I also checked to make sure there were no mismatches in the "total_ordered" columns in both *sales_by_sku* and *sales_report*:
```
SELECT sbs.*, sr.name, sr.total_ordered IS NOT NULL AS matching_sku
FROM sales_by_sku AS sbs
LEFT JOIN sales_report AS sr
ON sbs.productsku = sr.productsku
ORDER BY matching_sku
```
This confirmed that there were no mismatches in the data, save for those 8 records that are stored only in *sales_by_sku*. I then deleted the table for being a waste of space: 
```
DROP TABLE sales_by_sku;
```



