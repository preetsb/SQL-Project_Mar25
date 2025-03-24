# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
The ecommerce table is confusing but it's possible to see the valuable information in it and the reasons for the data collection. The goal is to clean up the table (as much as possible) and then try to find patterns and information that might help the business focus their marketing efforts. Just from first glance I could see that there was a lot of useful information about how the customers interacting with the website.  


## Process
Imported all the data into SQL from a .csv file. Before cleaning up, I examined the data with some preliminary queries (for example, checking to see if all values in a column were null or not). Tried to get a general sense of what is happening with the purchases and customer behaviour. 


## Results
Looks to be some sort of Amazon-type store that sells a lot of things, or maybe it's just Canadian Tire. 

CUSTOMER BEHAVIOUR? 
 
 I did notice that the top unique visitors by country did not match the top revenue earners which was intriguing and something I would explore more. The vast majority of the revenue is from the USA and they really like their custom decals.  


## Challenges 
The quality of the data (missing values, duplicates, confusing product names) was the primary challenge but worked with it as best as possible trying to find the story or at least patterns. Had to make a few assumptions about what some values meant, like that the 'google' and 'youtube' in product names are how they reached the site and it should be a seperate column and that some NULL values simply meant they didn't buy anything (I used that assumption with 'not available in data demoset' as well). 

It would have been useful to know what the values in columns like 'sentiment score' and 'ecommerce action type' mean as they could have provided some more insight but as it is, I just chose to eliminate them entirely since they were effectively meaningless. 


## Future Goals

