# Project : Superstore Sales Analysis
# Tool : MySQL / PostgreSQL
In this Portfolio project, l analyzed s superstore dataset.The primary goal of this project is to analyze the Superstore Sales dataset to identify key drivers of profitability and sales performance.
----------------------------------------------------------------------------------------------------------------------------
# Which product category are causing a loss?
**Sql query:**
```sql
SELECT
      Category,
      Sub_Category,
      ROUND(SUM(Profit),2) AS Total_Profit
FROM superstore
Group By Category, Sub_Category
HAVING SUM(Profit) < 0
ORDER BY Total_Profit ASC;

---------------------------------------------------------------------------------------------------------------------------
# Find customers who bought both Furniture and Technology in the same order?
**Sql query:**
```sql
SELECT DISTINCT
        a.Customer_Id,
        a.Customer_Name,
        a.Order_Id
FROM superstore a
JOIN superstore b
    ON a.Order_Id = b.Order_Id
WHERE a.Category = 'Furniture' AND b.Category = 'Technology';

--------------------------------------------------------------------------------------------------------------------------

# How do cumulative (running total) sales grow month by month for each year?
**Sql query:**
```sql
SELECT
       EXTRACT(YEAR FROM Order_Date) AS sales_year,
       EXTRACT(MONTH FROM Order_Date) AS sales_month,
       SUM(sales) AS monthly_sales
    FROM superstore
(
SELECT
       sales_year,
       sales_month,
       ROUND(SUM(Monthly_sales, 2) AS MONTHLY_SALES
       ROUND(SUM(Monthly_sales) OVER(PARTITION BY sales_year ORDER BY sales_month),2)  AS cumulative_sales
FROM MonthylySales
Order By sales_year, sales_month;

--------------------------------------------------------------------------------------------------------------------------------
## Categorize orders into High Profit, Medium Profit, and Loss and count them.
**sql query:**
```sql
SELECT
    CASE
        WHEN Profit > 100 THEN 'High Profit',
        WHEN Profit BETWEEN 0 AND 100 THEN 'MEDIUM PROFIT'
        ELSE 'LOSS'
     END AS PROFIT_CATEGORY ,
     COUNT(Row_Id) AS Order_Count,
     ROUND(SUM(Sales),2) AS Total_Sales
FROM superstore
GROUP BY
       CASE
         WHEN Profit > 100 THEN 'High Profit'
         WHEN Profit BETWEEN 0 AND 100 'Medium Profit'
         ESLE 'LOSS'
     END AS PROFIT_CATEGORY
  FROM superstore
) AS sub
GROUP BY Profit_Category;

------------------------------------------------------------------------------------------------------------------------------
## Find products that were ordered together in the same Order Id.
**sql query:**
```sql
   SELECT
        a.Order_Id,
        a.Product_Name AS Product_A,
        b.Product_Name AS Product_B
FROM superstore a
INNER JOIN superstore b
    on a.Order_Id = b.Order_Id
WHERE a.Product_Id < b.Product_id
LIMIT 5;

## Which customers have placed orders in two different ship modes(e.g, Standard Class AND First Class)?
**sql query:**
```sql
        SELECT DISTINCT
             a.Customer_Id,
             a.Customer_Name,
FROM superstore a
INNER JOIN superstore b
    ON a.Customer_Id = b.Customer_Id
WHERE a.Ship_Mode = 'First_Class' AND b.ship_name = 'Standard Class';
