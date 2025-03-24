### What are your risk areas? Identify and describe them.

The raw data in the ecommerce database is messy. 



QA Process:
Describe your QA process and include the SQL queries used to execute it.
```
SELECT (CAST(date AS DATE)), fullvisitorid, time, country, city, totaltransactionrevenue, transactions, timeonsite, pageviews, productquantity, productrevenue, productprice, productsku, v2productname 
FROM allsessions;
```

