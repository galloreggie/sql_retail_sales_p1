## Project Overview

**Project Title**: Retail Sales Analysis  
**Database**: `p1_retail_db`

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `p1_retail_db`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.
--SQL Retail Sales Analysis - p1
CREATE DATABASE sql_project_p2;

**Create Table**:
```sql
CREATE TABLE retail_sales
		(
			transactions_id	INT PRIMARY KEY,
			sale_date	DATE,
			sale_time	TIME,
			customer_id	INT,
			gender	VARCHAR(15),
			age	INT,
			category VARCHAR(15),	
			quantiy	INT,
			price_per_unit	FLOAT,
			cogs	FLOAT,
			total_sale FLOAT,
		);
```
## Data Cleaning

**select all**
```sql
SELECT * FROM retail_sales;
```

**verify count**
```sql
SELECT COUNT (*)
FROM retail_sales
```

**verify null rows**
```sql
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
	category IS NULL
	OR 
	cogs IS NULL
	OR 
	quantiy IS NULL
	OR 
	total_sale IS NULL;
```
	
**delete null rows**
```sql
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
	category IS NULL
	OR 
	cogs IS NULL
	OR 
	quantiy IS NULL
	OR 
	total_sale IS NULL;
```

## Data Exploration

**how many sales we have?**
```sql
SELECT COUNT (*) as total_sale FROM retail_sales
```

**how many unique customer we have?**
```sql
SELECT COUNT (DISTINCT customer_id) as total_sale FROM retail_sales
```

**how many categories we have?**
```sql
SELECT DISTINCT category FROM retail_sales
```


### DATA ANALYSIS AND KEY PROBLEMS AND ANSWERS

**1.Query to retrieve all columns for sales made on '2022-11-05'**
```sql
SELECT *
FROM retail_sales 
WHERE sale_date = '2022-11-05'
```

**2.Query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**
```sql
SELECT * 
FROM retail_sales 
WHERE 
	category = 'Clothing'
	AND
	TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
	AND
	quantiy >= 4
```

**3.Query to calculate the total sales (total_sale) for each category.**
```sql
SELECT 
	category,
	SUM(total_sale) AS net_sales
	FROM retail_sales
	GROUP BY 1
```

**4.Query to find the average age of customers who purchased items from the 'Beauty' category.**
```sql
SELECT 
	ROUND(AVG(age),2)
	FROM retail_sales
	WHERE category = 'Beauty'
```

**5.Query to find all transactions where the total_sale is greater than 1000.**
```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000
```

**6.Query to find the total number of transactions (transaction_id) made by each gender in each category.**
```sql
SELECT COUNT(transactions_id), gender, category
FROM retail_sales 
GROUP BY gender, category
```

**7.Query to calculate the average sale for each month. Find out best selling month in each year.**
```sql
SELECT * FROM
	(	
		SELECT 
		EXTRACT(YEAR FROM sale_date) as year,
		EXTRACT (MONTH FROM sale_date) as month,
		AVG(total_sale) as avg_sale,
		RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
		FROM retail_sales 
		GROUP BY 1,2
	) as t1 
WHERE rank = 1
```

**8.Query to find the top 5 customers based on the highest total sales.**
```sql
SELECT 
	customer_id,
	SUM (total_sale) as total_sales
	FROM retail_sales 
	GROUP BY 1
	ORDER by 2
	LIMIT 5
```

**9.Query to find the number of unique customers who purchased items from each category.**
```SQL
SELECT 
	category,
	COUNT(DISTINCT(customer_id)) as unique_customer
	FROM retail_sales 
	GROUP BY category
```

**10. Query to create each shift and number of orders.**
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
**End Project**

## Findings 

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.
	




