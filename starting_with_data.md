### Question 1: How many unique visitors are there on the site? 

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


### Question 2: How many unique visitors from each country? 
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

  
### Question 3: How many visitors actually bought something? What is the percentage?  
```
SELECT 
    DISTINCT fullvisitorid, 
    SUM(CAST(totaltransactionrevenue AS INTEGER) / 1000000) AS transaction_revenue
FROM allsessions
WHERE totaltransactionrevenue IS NOT NULL
GROUP BY fullvisitorid;
```
| FULL VISITOR ID     | TRANSACTION REVENUE |
|---------------------|---------------------|
| 0127755228974756313 | 103                 |
| 0223683667601132018 | 35                  |
| 0343104487250705794 | 111                 |
| 0348842070964414318 | 71                  |
| 0375962687766031488 | 27                  |
| 0395821106338763980 | 152                 |
| 0743094608412045835 | 51                  |
| 0751536527071005965 | 747                 |
| 0793185968741743873 | 17                  |

This query only returns 80 rows, out of over 14 thousand. So only .6% of visitors bought anything. (??) 

