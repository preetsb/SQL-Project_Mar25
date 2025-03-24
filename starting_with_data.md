###Question 1: How many unique visitors are there on the site? 

```
SELECT COUNT (fullvisitorid) AS visitors   
FROM allsessions
``` 
15134 total visitors 

```
SELECT COUNT (DISTINCT fullvisitorid) AS unique_visitors   
FROM allsessions
```
14223 UNIQUE visitors   


###Question 2: How many unique visitors from each country? 
```   
SELECT country, COUNT(DISTINCT fullvisitorid) AS unique_visitors    
FROM allsessions 
GROUP BY country  
ORDER BY unique_visitors DESC;
```
| COUNTRY        | UNIQUE_VISITORS |
|----------------|-----------------|
| United States  | 8118            |
| India          | 693             |
| United Kingdom | 641             |
| Canada         | 607             |
| Germany        | 321             |
| Japan          | 233             |
| Australia      | 219             |
| France         | 205             |
| Taiwan         | 160             |

  
###Question 3: What percentage of visitors did not buy anything? 

SQL Queries:

Answer:



Question 4: What percentage of vistors bought something? 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
