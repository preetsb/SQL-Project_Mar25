### What are your risk areas? Identify and describe them.

The raw data in the ecommerce database is messy and was difficult to work with. There were columns that were meaningless without more information and the number of assumptions I had to make was in itself a risk! 

There are duplicate data but it makes sense since someone will visit the website more than once (so probably requires a merge of some sort) to get the accurate data. Since I was unable to do this I simply chose to use distinct values and work with those - even though I sense it did not provide completely accurate information but it at least gave a sense of what customers were buying and where they are located. 



QA Process:
Describe your QA process and include the SQL queries used to execute it.

Focussed more on the allsessions table, since it contained a lot of the information available in other tables. 
There are 15134 rows in the table using the fullvisitorid column.  
```
SELECT COUNT(fullvisitorid) FROM allsessions
```

14223 when duplicates were eliminated 
```
SELECT COUNT(DISTINCT fullvisitorid) FROM allsessions
```

