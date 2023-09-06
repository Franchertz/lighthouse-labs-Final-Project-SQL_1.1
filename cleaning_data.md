What issues will you address by cleaning the data?

**Data Cleaning and Enhancement Considerations**

Based on the provided data, several issues necessitate attention during the data cleaning process:

1. **Addressing Missing Values:**
   - A critical aspect of data cleaning involves handling missing values in various columns such as "city," "country," "productsku," "unit_price," and others. Addressing these gaps is imperative to maintain data integrity and prevent potential errors in subsequent calculations and reporting.

2. **Ensuring Data Type Consistency:**
   - Maintaining data type consistency across columns is paramount. This entails verifying that numeric columns like "totaltransactionrevenue," "unit_price," and "total_ordered" have the appropriate data types (e.g., NUMERIC or INTEGER) to facilitate accurate calculations and analyses.

3. **Validating Data Integrity:**
   - Data cleaning encompasses the validation of categorical columns like "channelgrouping," "productsku," and "country" to ensure they contain valid and consistent values. Any anomalies or misspellings within these columns should be addressed to maintain data validity.

4. **Handling Duplicates:**
   - Identifying and appropriately managing duplicate rows within the tables is crucial. Eliminating duplicates helps prevent double-counting and enhances the precision of aggregate calculations and analyses.

5. **Standardizing Data Formats:**
   - Standardization of data formats, particularly in date-related columns such as "time" and "date," simplifies time-based analysis and facilitates meaningful comparisons. Consistent formatting ensures data uniformity.

6. **Preserving Referential Integrity:**
   - If foreign key relationships exist between tables, maintaining their referential integrity is paramount. Consistency in these relationships helps prevent data inconsistencies and safeguards against errors in queries and analyses.

7. **Data Normalization:**
   - In cases where data redundancy or denormalized data is observed, normalization techniques may be applied. Normalization reduces data duplication, enhancing data integrity and streamlining data management.

By meticulously addressing these data cleaning considerations, we aim to transform the raw data into a clean, consistent, and reliable foundation for subsequent analyses and insights.



Queries:
# Below, provide the SQL queries you used to clean your data.

## Update "city" and "country" columns with 'Other' where NULL or '(not set)'
```SQL
UPDATE all_sessions
SET city = 'Other'
WHERE city IS NULL OR city = '(not set)';
```

```SQL
UPDATE all_sessions
SET country = 'Other'
WHERE country IS NULL OR country = '(not set)';
```
## Convert "unit_price" column to NUMERIC data type
```SQL
ALTER TABLE analytics
ALTER COLUMN unit_price TYPE NUMERIC;
```
## Convert "total_ordered" column to INTEGER data type
```SQL
ALTER TABLE sales_by_sku
ALTER COLUMN total_ordered TYPE INTEGER;
```

## Convert "unit_price" column to NUMERIC data type
```SQL
ALTER TABLE analytics
ALTER COLUMN unit_price TYPE NUMERIC;
```

## Convert "total_ordered" column to INTEGER data type
```SQL
ALTER TABLE sales_by_sku
ALTER COLUMN total_ordered TYPE INTEGER;
```
## Identify duplicate rows based on all columns and assign a row number
```SQL
WITH duplicate_rows AS (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY fullvisitorid, channelgrouping, time, country, city, 
                              totaltransactionrevenue, transactions, pageviews, timeonsite, date,
                              pagetitle, searchkeyword, pagepathlevel1
                             ORDER BY fullvisitorid) AS rn
    FROM all_sessions
)
-- Delete duplicate rows, keeping only the first occurrence (row number = 1)
DELETE FROM all_sessions
WHERE (fullvisitorid, channelgrouping, time, country, city, 
       totaltransactionrevenue, transactions, pageviews, timeonsite, date,
       pagetitle, searchkeyword, pagepathlevel1) IN (
           SELECT fullvisitorid, channelgrouping, time, country, city, 
                  totaltransactionrevenue, transactions, pageviews, timeonsite, date,
                  pagetitle, searchkeyword, pagepathlevel1
           FROM duplicate_rows
           WHERE rn > 1
       );
```


