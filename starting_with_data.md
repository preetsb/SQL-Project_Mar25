Question 1: How many unique visitors are there on the site? 

SQL Queries:SELECT COUNT (fullvisitorid) AS unique_visitors
            FROM allsessions 

Answer: 15134 total visitors 
        14223 UNIQUE visitors 
        Interesting to see there are a lot of unique visitors as opposed to repeat customers - real growth opportunity. 



Question 2: How many unique visitors from each country? 

SQL Queries: SELECT country, COUNT(DISTINCT fullvisitorid) AS unique_visitors
              FROM allsessions 
              GROUP BY country
              ORDER BY unique_visitors DESC;

Answer: The top countries are USA, India, UK, Canada, and Germany. Interesting that the unique visitors and top revenue do not match (but I also realize how much data is missing).  



Question 3: 

SQL Queries:

Answer:



Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
