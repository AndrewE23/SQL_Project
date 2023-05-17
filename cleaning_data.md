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

>#4: In the *all_sessions* table, country is sometimes labelled "(not set)", and city is sometimes either "(not set)" or "not available in demo dataset". Any "(not set)" values can easily be replaced with NULL, while the other city string may have to be changed to "N/A" for ease of reading.

>#5: Product name entries in the *products*, *sales_reports*, and *all_sessions* tables sometimes have extraneous spaces, and for ease of access all product names can also be reduced to lower case. 

<hr>

## Queries
Below, provide the SQL queries you used to clean your data.<br>

### Issue #1: Remove Irrelevant Data
Focus: **all_sessions**<br>
```
ALTER TABLE all_sessions
DROP COLUMN searchkeyword,
DROP COLUMN productrefundamount,
DROP COLUMN itemquantity,
DROP COLUMN itemrevenue,
DROP COLUMN transactionrevenue,
DROP COLUMN productrevenue,
DROP COLUMN transactionid,
DROP COLUMN ecommerceaction_option,
DROP COLUMN productvariant,
DROP COLUMN currencycode;
```

Focus: **analytics**<br>
```
ALTER TABLE analytics
DROP COLUMN userid, -- All NULL
DROP COLUMN socialengagementtype; -- Only a single kind of value
```

### Issue #2: sales_by_sku
```
--The reason for this is thoroughly explained in QA.md.
DROP TABLE sales_by_sku;
```


### Issue #3: Monetary Amounts
All monetary amounts are multiplied by 1,000,000 (i.e. prices and revenues), meaning I had to convert them to a more standard format.

Update relevant analytics table numerics:
```
--Divide by 1,000,000
UPDATE analytics
SET unit_price = (unit_price / 1000000), revenue = (revenue / 1000000);
```
```
--Set to decimal precision to 2:
UPDATE analytics
SET unit_price = ROUND(unit_price, 2), revenue = ROUND(revenue, 2);
```
Update relevant all_sessions table numerics:
```
--Divide by 1,000,000
UPDATE all_sessions
SET totaltransactionrevenue = (totaltransactionrevenue / 1000000), productprice = (productprice / 1000000);
```
```
--Set to decimal precision to 2:
UPDATE all_sessions
SET totaltransactionrevenue = ROUND(totaltransactionrevenue, 2), productprice = ROUND(productprice, 2);
```
### Issue #4: Non-City/Country Names (all_sessions table)
As stated above, some country names are listed as "(not set)", while some city names are either "(not set)" or "not available in demo dataset." For ease of use, all "(not set)" values should be NULL, while all "not available in demo dataset" values should be "N/A".

NULL all instances of "(not set)":
```
--NULL unset countries
UPDATE all_sessions
SET country = NULL
WHERE country = '(not set)';
```
```
--NULL unset cities
UPDATE all_sessions
SET city = NULL
WHERE city = '(not set)';
```
Replace all instances of "not available in demo dataset":
```
UPDATE all_sessions
SET city = 'N/A'
WHERE city = 'not available in demo dataset';
```


### Issue #5: Name Formatting Errors
First, I checked for extra spaces in product names.
```
--Check for leading, extra spaces
--Note that I adapted this query for all_sessions and products as well
SELECT COUNT(name)
FROM sales_report
WHERE name ILIKE '%  %'
OR name ILIKE ' %'
```
Then I removed them:<br>
Remove extra spaces from "name" column in *sales_report*: 
```
--Remove leading spaces
UPDATE sales_report
SET name = TRIM(leading ' ' from name)
```
```
--Remove double spaces
UPDATE sales_report
SET name = regexp_replace(name, '  ', ' ')
```
Do the same for *products*: 
```
--Remove leading spaces
UPDATE products
SET name = TRIM(leading ' ' from name)
```
```
--Remove double spaces
UPDATE products
SET name = regexp_replace(name, '  ', ' ')
```
And then for *all_sessions*: 
```
--Remove leading spaces
UPDATE all_sessions
SET v2productname = TRIM(leading ' ' from v2productname)
```
```
--Remove double spaces
UPDATE all_sessions
SET v2productname = regexp_replace(v2productname, '  ', ' ')
```

