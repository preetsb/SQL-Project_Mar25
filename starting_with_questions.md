Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

``` 
SELECT country, SUM(CAST (unit_price AS integer)/1000000 * CAST (units_sold AS integer)) AS revenue2  
FROM analytics a
INNER JOIN allsessions al ON a.fullvisit_id = al.fullvisitorid   
WHERE units_sold IS NOT NULL   
GROUP BY country   
ORDER BY revenue2 DESC
```
```
SELECT city, SUM(CAST (unit_price AS integer)/1000000 * CAST (units_sold AS integer)) AS revenue2  
FROM analytics a
INNER JOIN allsessions al ON a.fullvisit_id = al.fullvisitorid   
WHERE units_sold IS NOT NULL   
GROUP BY city   
ORDER BY revenue2 DESC
```

Answer:   
Countries: United States, Czechia, Canada   
Cities: Mountain View, San Bruno, New York  



**Question 2: What is the average number of products ordered from visitors in each city and country?**

``` 
SELECT country, AVG(CAST(units_sold AS INTEGER)) AS avg_units_sold  
FROM analytics a
INNER JOIN allsessions al ON a.fullvisit_id = al.fullvisitorid  
WHERE units_sold IS NOT NULL  
GROUP BY country;
```
```
SELECT city, AVG(CAST(units_sold AS INTEGER)) AS avg_units_sold  
FROM analytics a
INNER JOIN allsessions al ON a.fullvisit_id = al.fullvisitorid  
WHERE units_sold IS NOT NULL  
GROUP BY city;
``` 

Answer: The majorty of orders are from Czechia and the United States,   



**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


```
SELECT country, ordered_quantity, v2productcategory  
FROM products p   
INNER JOIN allsessions al ON al.productsku = p.productsku   
WHERE ordered_quantity IS NOT NULL   
ORDER BY country, v2productcategory  
```

Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


``` 
WITH top_orders AS (  
SELECT country, name, SUM(CAST (ordered_quantity AS integer))AS total_ordered  
FROM products  
INNER JOIN allsessions ON products.productsku = allsessions.productsku  
GROUP BY country, name  
ORDER BY country, SUM(CAST (ordered_quantity AS integer)) DESC)  
  
, 
ranked_top_orders AS (  
SELECT country, name, total_ordered, RANK() OVER (PARTITION BY country ORDER BY total_ordered DESC) AS top_sellers  
FROM top_orders)   

SELECT *   
FROM ranked_top_orders   
WHERE top_sellers = 1
```

```
WITH top_orders AS (  
SELECT city, name, SUM(CAST (ordered_quantity AS integer))AS total_ordered  
FROM products  
INNER JOIN allsessions ON products.productsku = allsessions.productsku  
GROUP BY city, name  
ORDER BY city, SUM(CAST (ordered_quantity AS integer)) DESC)  
  
,  
ranked_top_orders AS (  
SELECT city, name, total_ordered, RANK() OVER (PARTITION BY city ORDER BY total_ordered DESC) AS top_sellers  
FROM top_orders)   

SELECT *   
FROM ranked_top_orders   
WHERE top_sellers = 1
```


Answer: I cannot see an obvious pattern but it's clear the majority of the revenue comes from custom decals and it's interesting that the USA's number one product is 'kick balls' and they also bring in the most revenue. Makes me wonder what is so special about these kick balls.  





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

```
Same queries as first question   

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
```

Answer: The top countries and cities clearly have the most impact on revenue but the USA is far, far ahead. Some historical data could be useful. 

![bar chart](https://github.com/user-attachments/assets/6180ca57-607f-45a9-9387-c01a300bcb2c)





