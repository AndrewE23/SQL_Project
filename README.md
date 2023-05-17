# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
Ultimately, my goal was to transform the data that has been made available to me into a dataset that can be relied on for delivering hard results and solid insights. What this means is it needs to be cleaned up; duplicates removed, useless info purged, and inconsistencies made consistent. After that, I want to be able to use the data to reliably answer different questions I may have like: What products are the most purchased? How many people actually buy things as opposed to simply view products? The final dataset should be coherent

## Process
### 1 - Profiling
The very first thing I needed to do was to form an accurate definition of what the dataset represents in the context of ecommerce, and apply data types as needed. It was easier to approach this on a per table basis:<br>
> *all_sessions*: This provides user statistics about how each user accessed the site; i.e. What was their point of entry and did they view or buy anything (or anything else)?
> 
> *analytics*: This provides user statistics about how each user used the site, for how long, and whether they purchased something or not. This only contains data for Q2 of 2017 (May-01 to August-01), but contains largely identical data for every page a user visited.
> 
> *products*: This is a list of products that the retailer has provided and how page visitors felt about said products.
> 
> *sales_report*: This is simply a report of the sales made by the retailer, although the timeframe is unknown. It is pretty similar to *products* in content.
> 
> *sales_by_sku*: This appears to be a trimmed sales report, organized by productsku.

Then, using a combination of LHL-provided and online reference materials for assistance, I devised a simple 5-step process for cleaning my data.<br>

### 2 - Validation
I kept three key main considerations in mind:<br>
**a. Data should be as complete as possible**. In short, I ran queries to see if I had NULL values, verified that unused contents were actually unused (or else barely used), and so forth.<br>
**b. Data should be unique.** If it isn't, then you run the risk if skewing or distorting any observations you make with the data available to you.<br>
**c. Data should be consistent.** Typos should be cleared, value formatting standardized, and so on. Basically, we want everything to be comprehensible at a glance and to know that it will continue making sense.<br>

### 3 - Data Cleaning
**a. Remove irrelevant data (from the source)**, such as empty columns.<br>
**b. Remove or filter duplicate data**, such as copied rows.<br>
**c. Remedy data inconsistencies**, such as matching data values or types that have been entered inconsistently.<br>
**d. Regulate data**, such as setting monetary values to one standard, dates to one format, etc.<br>
**e. Resolve issues with data gaps**, whether that means filling in what NULL values I can with the data available or else filtering related results out, depending on what is needed of the data.<br>
> Note: In a work or research setting, I could also attempt to resolve data gap problems by seeking out the missing data (i.e. collecting more data to fill in any gaps present). 

### 4 - Testing (and other considerations)
At this point, the goal is really to keep testing what data you have until any anomalies you have are dealth with (and even then keep testing)! Some tactics to assist with this may include: <br>
**a. Specific value checks, especially for NULLs**, so that you can verify the accuracy and integrity of your data.<br>
**b. Checking for invalid labels or categories**; your manager may not appreciate it if their title is stored as "manaher" or "walrus" rather than "manager".<br>
**c. Utilize data assertions**, maybe ensuring a user ID is properly stored and formatted or that a date makes sense.<br>

## Results
While this dataset has many unknowns in it, it still provides enough information to let us gauge, for example:<br>
a) What percentage of a country's orders make up the total number of products ordered.<br>
b) How people access the retailer's website.<br>
c) What kinds of products are popular in which countries, whether we are looking at specific item types, 
d) How recent purchasing trends compare with the retailer's cumulative sales records; ex. is it possible that sport bags were purchased far more in Argentina in mid-2017 than earlier in 2016?<br>

## Challenges 
With this being such a big project early in my data science training, there were times when I knew what to do but struggled to figure out exactly how to accomplish it without error. I wound up ignoring the *analytics* table as much as I could because, while I could identify easily that it was full of duplicate entries, I could not identify a key with which to differentiate them from one another; productsku wouldn't work because it was absent, which left unit_price as a semi-unique identifier and pageviews as an indicator of how many records there should be per sessionid... with the downside being that products can be priced identically while being distinct items, so it was not a foolproof plan for purging duplicates among over 40,000 records.

## Future Goals
I would try to normalize the table in a way where much of the data that is repeated (especially in the *analytics* table) would not be present; I do not need multiple copies of a record because someone looked at a particular item multiple times in a row. Likewise, I was able to delete the *sales_by_sku* table entirely because its contents were duplicated in *sales_report*, but that latter table could easily be combined with *products* to consolidate those tables together and remove more duplicate data from our dataset.
