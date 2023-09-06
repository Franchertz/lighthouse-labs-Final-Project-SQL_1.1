Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
SELECT 
  COALESCE(city, 'Other') AS city, 
  COALESCE(country, 'Other') AS country, 
  CAST(totaltransactionrevenue AS NUMERIC) AS total_revenue_numeric
FROM all_seasions
WHERE 
  totaltransactionrevenue IS NOT NULL
  AND city IS NOT NULL
  AND city <> ''
  AND country IS NOT NULL
  AND country <> ''
ORDER BY total_revenue_numeric DESC
LIMIT 1;



Answer:
City is not available in Dataset under Country column is United States with a total transaction of 1015480000




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
SELECT 
  city, 
  country, 
  ROUND(AVG(productquantity)::numeric, 2) AS average_products_ordered
FROM all_seasions
WHERE productquantity IS NOT NULL
GROUP BY city, country
ORDER BY average_products_ordered DESC
limit 5;



Answer:
We see an unknown city in the United Stated with the highest average of 10.58, followed by Madrid in Spain with an average of 10.00





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
SELECT 
  COALESCE(NULLIF(city, ''), 'Other') AS city, 
  COALESCE(NULLIF(country, ''), 'Other') AS country, 
  v2productcategory, 
  COUNT(*) AS order_count
FROM all_seasions
WHERE v2productcategory IS NOT NULL
  AND (city IS NOT NULL AND city != '(not set)')
  AND (country IS NOT NULL AND country != '(not set)')
  AND (city IS NOT NULL AND city != 'not available in demo dataset')
  AND (country IS NOT NULL AND country != 'not available in demo dataset')
GROUP BY city, country, v2productcategory
HAVING COUNT(*) > 10
ORDER BY country, city, order_count DESC
;



Answer:
The result of this query will give you a detailed breakdown of product category purchases for each city within each country, allowing you to analyze patterns in purchases across different locations.





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
WITH ranked_products AS (
  SELECT 
    COALESCE(NULLIF(city, ''), 'Other') AS city, 
    COALESCE(NULLIF(country, ''), 'Other') AS country, 
    v2productname, 
    COUNT(*) AS order_count,
    ROW_NUMBER() OVER (PARTITION BY city, country ORDER BY COUNT(*) DESC) AS ranked
  FROM all_seasions
  WHERE v2productname IS NOT NULL
    AND (city IS NOT NULL AND city != '(not set)')
    AND (country IS NOT NULL AND country != '(not set)')
	AND (city IS NOT NULL AND city != 'not available in demo dataset')
  	AND (country IS NOT NULL AND country != 'not available in demo dataset')
  GROUP BY city, country, v2productname
)
SELECT city, country, v2productname, order_count
FROM ranked_products
WHERE ranked = 1
ORDER BY country, city
;



Answer:
The results are ordered by country and city, allowing us to discern trends in the most sought-after products across various regions. This arrangement offers valuable insights into customer preferences, which can inform our marketing efforts, inventory management decisions, and product strategies. 





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
SELECT		city,
			country,
			SUM(totaltransactionrevenue::NUMERIC) AS total_revenue,
			v2productname
FROM		all_seasions
WHERE		totaltransactionrevenue IS NOT NULL
AND			country IS NOT NULL AND country != '(not set)'
AND			city IS NOT NULL AND city != '(not set)'
GROUP BY	city, country,v2productname
ORDER BY	total_revenue DESC
;



Answer:
We can summarize the revenue impact from each city and country by aggregating
the generated revenue figures. This summary allows us to understand the financial contributions of different regions, facilitating strategic decision-making and resource allocation to maximize our business outcomes.

Cities from United States generated the largest revenues. 

Reuseable shopping bag and Google Bongo Cupholder Bluetooth Speaker were to top selling products







