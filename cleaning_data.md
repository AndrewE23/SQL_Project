# What issues will you address by cleaning the data?
## Issues
What issues will you address by cleaning the data?

### Specific Issues
>#1a: There are many data fields that were largely or entirely empty (i.e. NULL), especially in the *all_sessions* table. Keeping them around would bog down loading and runtime speeds, as well as increase space used, and there would be little point anyway since data that is not available for use can not provide any meaningful insights. There were cases where a tiny fraction of a table's records had a value in these columns, but at that point there was still the issue of scale; I cannot extrapolate meaningful results from 30 values out of a total dataset of 15,000 records.

>#1b. Similarly, there were some columns where there was only one value stored, or else overwhelmingly one value and then a small portion that were NULL.<br> 
i) The "currencycode" column in *all_sessions* was almost entirely filled with 'USD', and if it wasn't then entries simply left it as NULL. As such, my assumption is that everything is being sold in USD and this column was redundant.<br> 
ii) The "socialengagementtype" column in *analytics* is similarly only ever one value.

>#2. The *sales_by_sku* table, which contained two columns ("productsku" and "total_ordered"), was entirely redundant. I compared it with the three other tables that include a sku/productsku column and found that every record was present in at least one other table, with the exception of 8 records that had no matches in any other table. Since the table is worthless

>#3. Monetary amounts in various columns were not formatted like currency, needing to be divided by 1,000,000 and rounded to two decimal points to better reflect what they are.

>#4: There is a lot of duplicate data in the *all_sessions* table, which had to be filtered out because duplicate data can skew any results I generate and call into question the quality of the data or my methods for parsing that data. 


### Other Consideratons
#1: A cursory glance at the *analytics* table initially made me think it was full of duplicates, but on closer inspection it turns out that many rows are differentiated by unique values in the "unit_price" column. This is something I address in the QA.md file.

<hr>

## Queries

Below, provide the SQL queries you used to clean your data.

### Issue #1: Remove Irrelevant Data
Focus: **all_sessions**<br>

I initially checked to see what these columns contained:<br>
>SELECT COUNT(\*) FROM all_sessions<br>
--WHERE searchkeyword IS NOT NULL --All are NULL<br>
--WHERE productrefundamount IS NOT NULL --All are NULL<br>
--WHERE itemquantity IS NOT NULL --All are NULL<br>
--WHERE itemrevenue IS NOT NULL --All are NULL<br>
--WHERE transactionrevenue IS NOT NULL --All but 4 are NULL<br>
--WHERE transactionid IS NOT NULL --All but 9 are NULL<br>
--WHERE searchkeyword IS NOT NULL --All but 31 are NULL<br>
The syntax provided here is because I checked each column individually, but wanted to keep ; un-comment one line at a time to replicate my process. 

I also wanted to check the "currencycode" column:<br>
>SELECT COUNT(*) AS totals --15134 total entries<br>
FROM all_sessions<br>
UNION<br>
SELECT COUNT (DISTINCT currencycode) --Only one distinct type<br>
FROM all_sessions<br>
UNION<br>
SELECT COUNT(currencycode) --14,862 entries with a set value ('USD')<br>
FROM all_sessions<br>

Column Deletion:<br>
>ALTER TABLE all_sessions<br>
DROP COLUMN searchkeyword,<br>
DROP COLUMN productrefundamount, -- All NULL<br>
DROP COLUMN itemquantity, -- All NULL<br>
DROP COLUMN itemrevenue, -- All NULL<br>
DROP COLUMN transactionrevenue, -- All but 4 are NULL<br>
DROP COLUMN transactionid, -- All but 9 are NULL<br>
DROP COLUMN ecommerce_option, -- All but 31 are NULL<br>
DROP COLUMN currencycode; -- Only 'USD' or null; safe assumption based on price and revenue values is that everything is sold in USD<br>


Focus: **all_sessions**<br>

>ALTER TABLE analytics<br>
DROP COLUMN userid, -- All NULL<br>
DROP COLUMN socialengagementtype; -- Only a single kind of value





