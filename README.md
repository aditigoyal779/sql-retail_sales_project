# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `retail_sales_db'

A beginner-level PostgreSQL project focused on analyzing retail sales data. The project covers data exploration, aggregation, and analytical queries to derive business insights. It was built as a learning exercise and extended with original queries to strengthen practical SQL skills.

## Objectives

The objective of this project is to gain hands-on experience with PostgreSQL by analyzing a retail sales dataset using SQL. This project aims to strengthen foundational SQL skills such as data querying, filtering, aggregation, grouping, and basic analysis.
By working on this project, the goal is to understand how SQL is used to answer real-world business questions related to sales performance, customer behavior, and time-based trends. The project also focuses on building confidence in writing efficient SQL queries and interpreting the results for data-driven insights.

## Project Structure

### 1. Database Setup

The project starts by creating a database named `retail_sales_db`.
A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
```

### 2. Data Exploration & Cleaning

- **Record Count**: Analyze the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
SELECT * FROM retail_sales 

SELECT 
     COUNT (*)
FROM retail_sales 

-- Data Cleaning --
SELECT * FROM retail_sales 
WHERE transactions_id IS NULL

SELECT * FROM retail_sales 
WHERE sale_date IS NULL

SELECT * FROM retail_sales 
WHERE sale_time IS NULL

SELECT * FROM retail_sales 
WHERE 
     transactions_id IS NULL
	 OR
	 sale_date IS NULL
	 OR
	 sale_time IS NULL
	 OR 
	 gender IS NULL
	 OR 
	 catEgory IS NULL
	 OR 
	 quantiy IS NULL
	 OR
	 cogs IS NULL
	 OR
	 total_sale IS NULL

--
DELETE FROM retail_sales
WHERE 
     transactions_id IS NULL
	 OR
	 sale_date IS NULL
	 OR
	 sale_time IS NULL
	 OR 
	 gender IS NULL
	 OR 
	 catEgory IS NULL
	 OR 
	 quantiy IS NULL
	 OR
	 cogs IS NULL
	 OR
	 total_sale IS NULL
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific questions I explored: 

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'
```

5. **Which customers have made more than one purchase**:
```sql
SELECT
    customer_id,
    COUNT(invoice_no) AS total_purchases
FROM retail_sales
GROUP BY customer_id
HAVING COUNT(invoice_no) > 1
ORDER BY total_purchases DESC;
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP 
    BY 
    category,
    gender
ORDER BY 1
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```

9. **What is the total sales amount per month.**:
```sql
SELECT
    DATE_TRUNC('month', sale_date) AS month,
    SUM(total_amount) AS monthly_sales
FROM retail_sales
GROUP BY month
ORDER BY month;
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift
```

## Findings

-Identified repeat customers through purchase frequency
-Observed category-wise differences in total sales
-Detected monthly sales trends using date-based analysis

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.


## Author - Aditi Goyal
B.Tech CSE student exploring database management and data analysis. This beginner-level project uses PostgreSQL to analyze retail sales data, covering essential SQL concepts such as SELECT statements, WHERE clauses, GROUP BY, aggregate functions, and basic joins.
email-id : aditigoyal779@gmail.com
LinkedIn : https://www.linkedin.com/in/aditi-goyal-50a9032b8?utm_source=share&utm_campaign=share_via&utm_content=profile&utm_medium=android_app




