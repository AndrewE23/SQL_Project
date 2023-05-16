# What issues will you address by cleaning the data?
## Issues
What issues will you address by cleaning the data?

### Issue #1<br>
#1: There are many data fields that largely or entirely empty (i.e. null), especially in the *all_sessions* table. Keeping them around would bog down loading and runtime speeds, as well as increase space used, and there would be little point anyway since a dataset that is not available for use can not provide any meaningful insights.<br>

#2: There is a lot of duplicate data in the *all_sessions* and a*nalytics* tables, which must be filtered out because duplicate data can skew any results I generate and call into question the quality of the data or my methods for parsing that data. While the biggest offender for duplicate entries was the *analytics* table, I found that the *sales_by_sku* table was particularly egregious because its entire dataset is present in the *sales_report* table save for 8 entries, which are not referenced in any other table and therefore can not provide any useful information.

#3. 



## Process

<hr>

# Queries

Below, provide the SQL queries you used to clean your data.

### Step 1: Remove Irrelevant Data<br>
Query:
> **ALTER TABLE** [table_name]<br>
> **DROP COLUMN** [column_name];

This was a standardized query where I filled in the [table_name] and [column_name] fields based on which tables I wanted modify. In my case:

**[table_name] = all_sessions**<br>
[column_name] =<br>
> a) searchkeyword................................(all null)<br>
b) productrefundamount.................(all null)<br>
c) itemquantity.....................................(all null)<br>
d) itemrevenue.....................................(all null)<br>
e) transactionrevenue........................(all but 4 are null)<br>
f) transactionid.....................................(all but 9 are null)<br>
g) ecommerce_option.......................(all but 31 are null)<br>

**[table_name] = analytics**<br>
[column_name] =<br>
> a) userid..................................................(all null)<br>
b) socialengagementtype................(only one value)





