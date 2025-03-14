Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries: 
SELECT country, SUM(CAST (unit_price AS integer)/1000000 * CAST (units_sold AS integer)) AS revenue2
FROM analytics
INNER JOIN allsessions ON analytics.fullvisit_id = allsessions.fullvisitorid 
WHERE units_sold IS NOT NULL 
GROUP BY country 
ORDER BY revenue2 DESC

SELECT city, SUM(CAST (unit_price AS integer)/1000000 * CAST (units_sold AS integer)) AS revenue2
FROM analytics
INNER JOIN allsessions ON analytics.fullvisit_id = allsessions.fullvisitorid 
WHERE units_sold IS NOT NULL 
GROUP BY city 
ORDER BY revenue2 DESC



Answer: Countries: United States, Czechia, Canada 
        Cities: Mountain View, San Bruno, New York



**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
SELECT country, AVG(CAST(units_sold AS INTEGER)) AS avg_units_sold
FROM analytics
INNER JOIN allsessions ON analytics.fullvisit_id = allsessions.fullvisitorid
WHERE units_sold IS NOT NULL
GROUP BY country;

SELECT city, AVG(CAST(units_sold AS INTEGER)) AS avg_units_sold
FROM analytics
INNER JOIN allsessions ON analytics.fullvisit_id = allsessions.fullvisitorid
WHERE units_sold IS NOT NULL
GROUP BY city;


Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
SELECT country, ordered_quantity, v2productcategory
FROM products 
INNER JOIN allsessions ON allsessions.productsku = products.productsku 
WHERE ordered_quantity IS NOT NULL 
ORDER BY country, v2productcategory


Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







