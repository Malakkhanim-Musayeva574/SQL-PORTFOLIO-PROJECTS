# E-Commerce Sales Analysis SQL
## Project
In this portfolio project, l analyzed a cleaned e-commerce dataset containing retail transactions.The goal is extract actionable business insights regarding product performance, regional sales trends,
and profitability using advanced SQL techniques.
------------
# Business questions and solutions with SQL

# Which product categories generate the highest sales volume and profit?
**Sql query:**
```sql
SELECT
    product_category,
    SUM(quantity_sold) AS total_units_sold,
    ROUND(SUM(total_revenue), 2) AS total_sales_revenue,
    ROUND(SUM(profit), 2) AS total_net_profit
FROM commerce
GROUP BY product_category
ORDER BY total_sales_revenue DESC;


# What are the the preffered payment methods across different global regions?
**Business goal:** Understand customer payment behavior across  different global regions to improve checkout experiences an localization.
**SQL query:**
```sql
SELECT 
    customer_region,
    payment_method,
    COUNT(order_id) AS total_transactions,
    ROUND(SUM(total_revenue), 2) AS regional_revenue
FROM commerce
GROUP BY customer_region, payment_method
ORDER BY customer_region, regional_revenue DESC;


# What are our top-tier single orders based on profitability?
**Business goal:** Isolate the top single orders based on profitability using window functions to understand high-value customer purchasing behavior.

**SQL query:**
```sql
WITH RankedOrders AS (
    SELECT 
        order_id,
        product_category,
        total_revenue,
        profit,
        DENSE_RANK() OVER (ORDER BY profit DESC) AS profit_rank
    FROM commerce
)
SELECT 
    order_id,
    product_category,
    total_revenue,
    profit
FROM RankedOrders
WHERE profit_rank <= 5;

* **Language :** SQL
* **Key Concepts :** CTEs, Window Functions,Aggregations,Data Sorting.
