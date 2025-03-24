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
| COUNTRY        | REVENUE2  |   |   |   |   |   |   |   |   |
|----------------|-----------|---|---|---|---|---|---|---|---|
| United States  | 11832011  |   |   |   |   |   |   |   |   |
| Czechia        | 12534     |   |   |   |   |   |   |   |   |
| Canada         | 4202      |   |   |   |   |   |   |   |   |
| Mexico         | 2320      |   |   |   |   |   |   |   |   |
| Hong Kong      | 2291      |   |   |   |   |   |   |   |   |
| Japan          | 1492      |   |   |   |   |   |   |   |   |
| Sweden         | 688       |   |   |   |   |   |   |   |   |
| United Kingdom | 658       |   |   |   |   |   |   |   |   |
| Taiwan         | 598       |   |   |   |   |   |   |   |   |


```
SELECT city, SUM(CAST (unit_price AS integer)/1000000 * CAST (units_sold AS integer)) AS revenue2  
FROM analytics a
INNER JOIN allsessions al ON a.fullvisit_id = al.fullvisitorid   
WHERE units_sold IS NOT NULL   
GROUP BY city   
ORDER BY revenue2 DESC
```

| CITY          | REVENUE2  |   |   |   |   |   |   |   |   |
|---------------|-----------|---|---|---|---|---|---|---|---|
| NULL          | 9413957   |   |   |   |   |   |   |   |   |
| Mountain View | 1698297   |   |   |   |   |   |   |   |   |
| San Bruno     | 382538    |   |   |   |   |   |   |   |   |
| New York      | 117007    |   |   |   |   |   |   |   |   |
| Chicago       | 90331     |   |   |   |   |   |   |   |   |
| Sunnyvale     | 39759     |   |   |   |   |   |   |   |   |
| Charlotte     | 16241     |   |   |   |   |   |   |   |   |
| Kirkland      | 14571     |   |   |   |   |   |   |   |   |
| Austin        | 11043     |   |   |   |   |   |   |   |   |

(NULL is unknown cities and are from many countries, so the top city is Mountain View) 


**Question 2: What is the average number of products ordered from visitors in each city and country?**

``` 
SELECT country, AVG(CAST(units_sold AS INTEGER)) AS avg_units_sold  
FROM analytics a
INNER JOIN allsessions al ON a.fullvisit_id = al.fullvisitorid  
WHERE units_sold IS NOT NULL  
GROUP BY country;
```
| COUNTRY     | AVG_UNITS_SOLD         |   |   |   |   |   |   |   |   |
|-------------|------------------------|---|---|---|---|---|---|---|---|
| Australia   | 1.00000000000000000000 |   |   |   |   |   |   |   |   |
| Austria     | 1.00000000000000000000 |   |   |   |   |   |   |   |   |
| Belarus     | 1.00000000000000000000 |   |   |   |   |   |   |   |   |
| Belgium     | 1.00000000000000000000 |   |   |   |   |   |   |   |   |
| Bulgaria    | 1.5000000000000000     |   |   |   |   |   |   |   |   |
| Canada      | 1.5934065934065934     |   |   |   |   |   |   |   |   |
| Chile       | 1.00000000000000000000 |   |   |   |   |   |   |   |   |
| Colombia    | 1.00000000000000000000 |   |   |   |   |   |   |   |   |
| Czechia     | 15.1818181818181818    |   |   |   |   |   |   |   |   |

```
SELECT city, AVG(CAST(units_sold AS INTEGER)) AS avg_units_sold  
FROM analytics a
INNER JOIN allsessions al ON a.fullvisit_id = al.fullvisitorid  
WHERE units_sold IS NOT NULL  
GROUP BY city;
```
| CITY      | AVG_UNITS_SOLD         |
|-----------|------------------------|
| Ahmedabad | 1.00000000000000000000 |
| Ann Arbor | 1.00000000000000000000 |
| Atlanta   | 3.2727272727272727     |
| Austin    | 3.6071428571428571     |
| Bangkok   | 1.00000000000000000000 |
| Berlin    | 1.00000000000000000000 |
| Bogota    | 1.00000000000000000000 |
| Cambridge | 2.6923076923076923     |
| Charlotte | 4.6467065868263473     |

(Table only shows first ten, in alphabetical order)    


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**

```
SELECT country, ordered_quantity, v2productcategory  
FROM products p   
INNER JOIN allsessions al ON al.productsku = p.productsku   
WHERE ordered_quantity IS NOT NULL   
GROUP BY country, ordered_quantity, v2productcategory  
ORDER BY country ASC;   
```
```
SELECT city, ordered_quantity, v2productcategory  
FROM products p   
INNER JOIN allsessions al ON al.productsku = p.productsku   
WHERE ordered_quantity IS NOT NULL   
GROUP BY city, ordered_quantity, v2productcategory  
ORDER BY city ASC; 
```
I cannot see any obvious pattern, and it's hard because the product categories don't make much sense. Sometimes it is general apparel, other times it gets more specific (men's t-shirst).  


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
| COUNTRY   | NAME                                          | TOTAL ORDERED |   |
|-----------|-----------------------------------------------|---------------|---|
| (not set) | Custom Decals                                 | 3786          |   |
| Albania   | 22 oz  Bottle Infuser                         | 1465          |   |
| Algeria   | Twill Cap                                     | 1429          |   |
| Argentina | SPF-15 Slim & Slender Lip Balm                | 3682          |   |
| Armenia   | Windup Android                                | 1351          |   |
| Australia | Custom Decal                                  | 18930         |   |
| Austria   | Custom Decals                                 | 3786          |   |
| Bahamas   | Twill Cap                                     | 917           |   |
| Bahrain   | Men's 100% Cotton Short Sleeve Hero Tee White | 528           |   |

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
| CITY      | NAME                                         | TOTAL ORDERED |   |
|-----------|----------------------------------------------|---------------|---|
| (not set) | Custom Decals                                | 34074         |   |
| Adelaide  | Men's Watershed Full Zip Hoodie Grey         | 54            |   |
| Ahmedabad | Leatherette Notebook Combo                   | 1148          |   |
| Akron     | Men's 100% Cotton Short Sleeve Hero Tee Navy | 15            |   |
| Amsterdam | Hard Cover Journal                           | 1330          |   |
| Ann Arbor | Foam Can and Bottle Cooler                   | 2442          |   |
| Antalya   | Tube Power Bank                              | 0             |   |
| Antwerp   | Colored Pencil Set                           | 269           |   |
| Appleton  | Aluminum Handy Emergency Flashlight          | 66            |   |


Thare just the first ten countries when put in alphabetical order. The only thing that seems to be important is that the two biggest sellers are Kick Ball and Custom Decals (when total ordered is sorted from highest to lowest), and this is true for a lot of countries. The same applies to the cities except for Palo Alto and Sunnyvale which have a strong demand for outdoor security cameras - which is interesting and I wonder why. 

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

Answer: I assume that the top countries and cities with the highest transaction revenues have the most impact on revenue but the USA is far, far ahead. Some historical data could be useful to see how demand has changed in different locations. 

![bar chart](https://github.com/user-attachments/assets/6180ca57-607f-45a9-9387-c01a300bcb2c)





