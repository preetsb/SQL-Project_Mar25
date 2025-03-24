## What issues will you address by cleaning the data?

There are too many issues to address so I went with the most obvious (and that were relevant to the questions). 
* The formatting of the dates is in Unix code instead of the traditional date format 
* There is a lot of missing data that makes the analysis less useful 
	* Missing data between cities and countries makes geographic information more complicated to parse 	so I’ll try to deal with the null values and create some consistency between “not available in demo set” 	and “null” 
	* The confusion with the product names. I’m assuming that the products are not Google or YouTube 
	branded but instead this is how they led to the product. I’ll create a new column if it makes sense. 
	* There are some columns that are meaningless without more information such as 	ecommeractionaction_type, there’s no way to find out what the numbers mean 
	*The null values are useful in some tables (like analytics) because then you can see how many 	people visited the site, how long they stayed, and if they bought anything. 
	


Queries:
Below, provide the SQL queries you used to clean your data.

### allsessions   
Filtered the table for distinct fullvisitorid and kept the relevant columns 
```
SELECT DISTINCT fullvisitorid,
		time, 
		country,
 		city,
		totaltransactionrevenue,
		transactions,
		timeonsite,
		pageviews,
		date,
		productquantity,
		productrevenue,
		 productprice,
		productsku, 
		v2productname 
FROM allsessions;
```

Changed ‘not available in demo dataset’ to NULL 
```
UPDATE allsessions 
	SET city = NULL 
WHERE city = 'not available in demo dataset'
```

Changed date column to standard format 
```
SELECT CAST(date AS DATE) FROM allsessions
```
Then filtered for the relevant data 
```
SELECT (CAST(date AS DATE)), fullvisitorid, time, country, city, totaltransactionrevenue, transactions, timeonsite, pageviews, productquantity, productrevenue, productprice, productsku, v2productname 
FROM allsessions;
```

### analytics  
Changed unit_price to integer, performed calculation to make it make more sense. 
```
SELECT(CAST (unit_price AS integer)/1000000) AS unit_price
FROM analytics
```

### products
Dropped sentimentscore and sentimentmagnitude from Products table because I have no information what they indicate
```
ALTER TABLE products 
DROP COLUMN sentimentscore; 
```
```
ALTER TABLE products 
DROP COLUMN sentimentmagnitude; 
```

### sales_report
Dropped sentiment_score and sentimentmagnitude from sales_report table for same reason as products table (and sentiment_score column is titled different from products table) 
```
ALTER TABLE sales_report 
DROP COLUMN sentiment_score; 
```
```
ALTER TABLE sales_report 
DROP COLUMN sentimentmagnitude; 
```

 
