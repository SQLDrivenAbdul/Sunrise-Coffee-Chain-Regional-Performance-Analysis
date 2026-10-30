# Sunrise Coffee Chain: Regional Profitability Analysis

## Project Overview

Sunrise Coffee Chain is a growing coffee retail business with 60 stores operating across 12 regions in the United Kingdom. Despite experiencing steady growth over the past five years, differences in store and regional profitability have raised concerns within the commercial team.

This project evaluates the company's financial performance, focusing on store-level and regional profitability. The analysis seeks to identify the most and least profitable stores, examine the factors driving profitability differences, and uncover opportunities to improve financial performance.

The findings will provide actionable insights to support the Chief Commercial Officer's regional prioritization and site selection decisions for the 2025 expansion programme.

## Business Problem

The commercial team suspects that profitability varies significantly across regions, but lacks sufficient insight into which stores are generating the strongest returns and which are underperforming financially.

Management needs a clearer understanding of store-level and regional profitability, including the factors influencing differences in financial performance, to make informed decisions about future investments and expansion priorities.

## Project Objectives

- Evaluate store-level and regional profitability across 60 stores and 12 regions.
- Identify the most and least profitable stores.
- Compare revenue, operating costs, and profit margins across regions.
- Examine the factors driving differences in store profitability.
- Identify regions with strong profitability and those requiring operational improvement.
- Provide data-driven recommendations to support the 2025 site selection programme.

## Tools and Technologies

- **SQL:** Data exploration, querying, and analysis of store-level and regional profitability.
- **Power BI:** Data visualization and dashboard development to communicate findings and business insights.


## SQL-Based Exploratory Data Analysis

The goal of this project is to understand store-level and regional profitability. I have therefore adopted a top-down approach, starting at the regional level before drilling down into individual stores.

To begin, the query below calculates each region's total revenue, net profit, and net profit margin (the percentage of revenue retained as profit).

```sql
SELECT 
	r.region,
	SUM(l.total_monthly_revenue_gbp) AS revenue,
	SUM(net_profit_gbp) AS profit,
	ROUND(SUM(net_profit_gbp)/SUM(l.total_monthly_revenue_gbp)*100,2) AS profit_margin
FROM fact_monthly_sales AS l
INNER JOIN dim_store AS r
ON l.store_id = r.store_id
GROUP BY  r.region
ORDER BY profit_margin DESC
```
## Result

