# Sales Performance Analysis

This project focuses on analyzing sales performance to derive actionable insights regarding product success, revenue trends, and customer purchasing behaviors.

## Project Overview

The dataset includes three main tables: 
- **`products`**: Contains information about each product, including their categories and identifiers.
- **`transactions`**: Records details of each transaction, such as transaction dates, amounts spent, and quantities sold.
- **`customers`**: Provides data on customer identifiers and related information.

## Analysis Insights

1. **Top-Performing Products**  
   **Purpose**: Identify the highest-selling products based on total sales.  
   **Insight**: Highlights products with sales exceeding the average, showcasing their popularity.

```sql
   SELECT product_id, product_name, total_sales
FROM (
    SELECT p.product_id, p.product_name, SUM(t.amount_spent) AS total_sales
    FROM products p
    JOIN transactions t ON p.product_id = t.product_id
    GROUP BY p.product_id, p.product_name
) AS product_sales
WHERE total_sales > (
    SELECT AVG(total_sales) FROM (
        SELECT SUM(t.amount_spent) AS total_sales
        FROM products p
        JOIN transactions t ON p.product_id = t.product_id
        GROUP BY p.product_id
    ) AS avg_sales
);
```
2. **Sales Trends Over Time**
**Purpose**: Analyze monthly sales performance and track trends over time.
**Insight**: Enables businesses to understand seasonal sales patterns.

 ```sql
WITH monthly_sales AS (
    SELECT DATE_TRUNC('month', transaction_date) AS sales_month, SUM(amount_spent) AS total_sales
    FROM transactions
    GROUP BY sales_month
)
SELECT sales_month, total_sales
FROM monthly_sales
ORDER BY sales_month;
   ```

3. **Top Products by Quantity Sold**  
   **Purpose**: Determine which products have the highest sales volume.  
   **Insight**: Provides insights into product demand and customer preferences.
    ```sql
    SELECT p.product_name, SUM(t.quantity_sold) AS total_quantity_sold
    FROM products p
    JOIN transactions t ON p.product_id = t.product_id
    GROUP BY p.product_name
    ORDER BY total_quantity_sold DESC;
    '''

4. **Average Transaction Amount by Product**  
   **Purpose**: Calculate the average amount spent per transaction for each product.  
   **Insight**: Useful for refining pricing strategies based on consumer behavior.
   ```sql
   SELECT p.product_name, AVG(t.amount_spent) AS avg_transaction
FROM products p
JOIN transactions t ON p.product_id = t.product_id
GROUP BY p.product_name
ORDER BY avg_transaction DESC;
'''

5. **Total Revenue by Product Category**  
   **Purpose**: Assess the total revenue generated across different product categories.  
   **Insight**: Identifies which categories are the most profitable, aiding strategic decision-making.
   ```sql
   WITH category_revenue AS (
    SELECT p.product_category, SUM(t.amount_spent) AS total_revenue
    FROM products p
    JOIN transactions t ON p.product_id = t.product_id
    GROUP BY p.product_category
)
SELECT product_category, total_revenue
FROM category_revenue
ORDER BY total_revenue DESC;
'''

6. **Customer Purchases Over Time**  
   **Purpose**: Monitor cumulative spending for each customer over time.  
   **Insight**: Offers insights into customer loyalty and evolving purchasing patterns.
   ```sql
   SELECT t.customer_id, t.transaction_date, 
       SUM(t.amount_spent) OVER (PARTITION BY t.customer_id ORDER BY t.transaction_date) AS cumulative_spending
   FROM transactions t
   ORDER BY t.customer_id, t.transaction_date;
   '''

## How to Use

You can run these SQL queries in any database management system that supports SQL. Just ensure you have the necessary tables and data uploaded.

The SQL queries included in this project demonstrate various advanced techniques such as:

- **Correlated Subqueries**: Used to identify top-performing products based on sales.
- **Common Table Expressions (CTEs)**: Employed for better organization of complex queries and to track sales trends.
- **Window Functions**: To calculate cumulative spending per customer and other insights.
- **Seasonal Analysis**: Understanding product performance in different seasons.

## Contact

Feel free to connect with me for any inquiries or collaboration opportunities.

