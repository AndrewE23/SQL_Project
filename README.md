# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
Ultimately, my goal is to transform the data that has been made available to me into a dataset that can be relied on for delivering hard results and solid insights. What this means is it needs to be cleaned up; duplicates removed, useless info purged, and inconsistencies made consistent. After that, I want to be able to use the data to reliably answer different questions I may have like: What products are the most purchased? How many people actually buy things as opposed to simply view products? 

## Process
### 1 - Assessment
The very first thing I needed to do was to form an accurate definition of what the dataset represents in the context of ecommerce. It was easier to approach this on a per table basis:<br>
> *all_sessions*: This provides user statistics about how each user accessed the site; i.e. What was their point of entry and did they view or buy anything (or anything else)?
> 
> *analytics*: This provides user statistics about how each user used the site, for how long, and whether they purchased something or not. This only contains data for Q2 of 2017 (May-01 to August-01), but contains largely identical data for every page a user visited.
> 
> *products*: This is a list of products that the retailer has provided and how page visitors felt about said products.
> 
> *sales_report*: This is simply a report of the sales made by the retailer, although the timeframe is unknown.
> 
> *sales_by_sku*: This appears to be a trimmed sales report, organized by productsku.

Then, using a combination of LHL-provided and online reference materials for assistance, I devised I came up with a simple 5-step process for cleaning my data.<br>

### 2 - Data Validation & Cleaning
**a. Remove irrelevant data (from the source)**, such as empty columns.<br>
**b. Remove or filter duplicate data**, such as copied rows.<br>
**c. Remedy data inconsistencies**, such as matching data values or types that have been entered inconsistently.<br>
**d. Regulate data**, such as setting monetary values to one standard, dates to one format, etc.<br>
**e. Resolve issues with data gaps**, whether that means filling in what NULL values I can with the data available or else filtering related results out, depending on what is needed of the data.<br>
> Note: In a work setting, I could also attempt to resolve data gap problems by seeking out the missing data (i.e. collecting more data to fill in any gaps present). This would be a validation stage, but is not part of my process for this project.


## Results
(fill in what you discovered this data could tell you and how you used the data to answer those questions)

## Challenges 
(discuss challenges you faced in the project)

## Future Goals
(what would you do if you had more time?)
