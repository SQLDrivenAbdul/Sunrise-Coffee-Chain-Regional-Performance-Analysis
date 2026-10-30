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
### Result
![Regional Profitability Query Result](docs/Result%20SR1.JPG)

The results show all regions and their respective performance indicators. Interestingly, London, the region with the highest revenue, **is the only region with a profit margin below 10%**, recording just 8.45%.

### Why Is This Happening?

This raises a further question: why is London's profit margin so low despite generating the highest revenue?

To investigate, I will examine the expenses across the regions to understand where the money is going. The query shows each operating cost as a percentage of regional revenue, including cost of goods sold, rent, salaries, and loyalty redemption costs.

```sql
SELECT
	region,
	ROUND(SUM(l.total_monthly_cogs_gbp)/SUM(l.total_monthly_revenue_gbp)*100,2) AS cogs,
	ROUND(SUM(l.monthly_rent_gbp)/SUM(l.total_monthly_revenue_gbp)*100,2) AS rent,
	ROUND(SUM(l.monthly_staffing_cost_gbp)/SUM(l.total_monthly_revenue_gbp)*100,2) AS salary,
	ROUND(SUM(l.loyalty_redemption_cost_gbp)/SUM(l.total_monthly_revenue_gbp)*100,2) AS loyalty_redemption_cost
FROM fact_monthly_sales AS l
INNER JOIN dim_store AS r
ON l.store_id = r.store_id
GROUP BY region
```

### Result
![Regional Operating Cost Breakdown](https://raw.githubusercontent.com/SQLDrivenAbdul/Sunrise-Coffee-Chain-Regional-Performance-Analysis/main/docs/Result%20SR2.JPG)

The result above shows that London's rent ratio is notably higher than every other region, while its salary ratio is not the highest. This makes rent a strong area to investigate further.

### Let's go!!!
Now, let's investigate the average monthly rent  in the regions.

```sql
SELECT
	region,
	ROUND(AVG(l.monthly_rent_gbp),2) AS average_monthly_rent
FROM fact_monthly_sales AS l
INNER JOIN dim_store AS r
ON l.store_id = r.store_id
GROUP BY region
ORDER BY average_monthly_rent DESC
```

### Result
![Regional Profitability Analysis](https://raw.githubusercontent.com/SQLDrivenAbdul/Sunrise-Coffee-Chain-Regional-Performance-Analysis/main/docs/Result%20SR3.JPG)

Turns out the stores in London an average of £11,744.12 on rent making it the region that spends the most on rent, followed by South-East with £8,293.26 GBP. Interestingly, the difference of London and South-East average monthly rent is £83.38% of West-Midlands's average monthly rent.
