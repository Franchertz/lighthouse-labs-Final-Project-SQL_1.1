Question 1: 
What are the top countries and cities by total transaction revenue?

```SQL Queries:
SELECT		country,
			city,
			SUM(ROUND(totaltransactionrevenue::numeric/1000000, 2)) AS total_revenue
FROM		all_seasions
WHERE		totaltransactionrevenue IS NOT NULL
AND			country IS NOT NULL AND country != '(not set)'
AND			city IS NOT NULL AND city != '(not set)'
GROUP BY	country, city
ORDER BY total_revenue DESC
LIMIT 10
;
```
```SQL
UPDATE all_sessions
SET city = 'Other'
WHERE city IS NULL OR city = '(not set)';
```
#### Answer: 
A city in USA city unknown had the largest total revenue just over 6million




## Question 2: 
What are the most viewed pages and their associated revenue

```SQL Queries:
SELECT			als.pagetitle,
				SUM(als.pageviews) AS total_pageviews,
				ROUND(SUM(a.revenue) / 1000000, 2) AS revenue_millions
FROM			all_seasions als
JOIN			analytics a
USING			(pageviews)
WHERE			a.revenue IS NOT NULL
GROUP BY		als.pagetitle
ORDER BY		total_pageviews DESC
Limit 5;
```
#### Answer:
The office, Nest-USA and "Men's T-Shirts | Apparel | Google Merchandise Store" had the largest pageview with over 300,000 views and also generated 9million, 8million and 9million in revenue respectively.



## Question 3: 
How did studying visitor behavior metrics, such as session duration, page views, and search keywords

```SQL Queries:
SELECT		fullvisitorid, 
       		ROUND(AVG(pageviews)) AS avg_pageviews,
	   		ROUND(EXTRACT(EPOCH FROM AVG(timeonsite))::numeric) AS avg_timeonsite
FROM		all_seasions
WHERE		fullvisitorid IS NOT NULL
GROUP BY	fullvisitorid
HAVING		ROUND(EXTRACT(EPOCH FROM AVG(timeonsite))::numeric) IS NOT NULL 
ORDER BY	avg_timeonsite DESC
;
```
#### Answer:
A thorough examination of visitor behavior metrics, which encompass session duration, page views, and search keywords, yielded invaluable insights into the way visitors engaged with our website. Through the scrutiny of these metrics, we obtained a deeper comprehension of user preferences and behaviors, enabling us to pinpoint areas in need of enhancement within the website's user experience. 

For instance, we could readily pinpoint pages exhibiting high bounce rates or low engagement and subsequently optimize their content to render them more enticing to our visitors. Moreover, the analysis of search keywords unveiled popular topics of interest, empowering us to craft targeted content that deeply resonates with our audience.

In summary, this data-centric approach culminated in improvements across the website's design, content, and navigation, resulting in heightened visitor engagement and an overall more gratifying user experience.



Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
