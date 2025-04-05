/*
This query calculates the retention rates for 1, 2, and 3 months based on the time between first and second purchases.
*/

WITH months AS (           -- Grouping customers by the month of their first and second purchases using cohort_analysis table
  SELECT
    customer_id
    ,DATE(DATE_TRUNC('MONTH', first_purchase)) AS first_month
    ,DATE(DATE_TRUNC('MONTH', second_purchase)) AS second_month
  FROM 
    cohort_analysis
),

tt AS (                    -- Calculated the number of months between first and second purchases
  SELECT
    customer_id
    ,first_month AS month_group
    ,MONTHS_BETWEEN(second_month, first_month) AS months_between
  FROM 
    months
)

-- Final selection calculates the retention rates for 1, 2, and 3 months based on the time between the first and second purchases using CASE statements.
SELECT
  month_group
  ,COUNT(DISTINCT customer_id) AS total_customers
  ,ROUND(COUNT(DISTINCT CASE WHEN months_between <= 1 THEN customer_id ELSE NULL END) / COUNT(DISTINCT customer_id), 2) AS retention_rate_1m
  ,ROUND(COUNT(DISTINCT CASE WHEN months_between <= 2 THEN customer_id ELSE NULL END) / COUNT(DISTINCT customer_id), 2) AS retention_rate_2m
  ,ROUND(COUNT(DISTINCT CASE WHEN months_between <= 3 THEN customer_id ELSE NULL END) / COUNT(DISTINCT customer_id), 2) AS retention_rate_3m
FROM 
  tt
GROUP BY 
  month_group
ORDER BY 
  month_group
;
