


# SQL Retail Sale Analysis
'''sql
CREATE DATABASE sql_project_p3;
'''
### CREATE TABLE
'''sql
CREATE  TABLE retail_sales

 (
transactions_id	int,
sale_date	date,
sale_time	time,
customer_id	int,
gender	varchar(15),
age	int,
category	varchar	(15),
quantiy	int,
price_per_unit FLOAT,	
cogs	FLOAT,
total_sale FLOAT
)
'''
### Seeing that the table is populated 
'''sql
SELECT * FROM retail_sales
'''

## DATA EXPLORATION 

### How many sales we have?
'''sql
Select Count(*) as total_sale FROM retail_sales 
'''

### How many unique customers we have?
'''sql
Select Count(DISTINCT customer_id) as total_sale From retail_sales
'''
### How many unique category we have?
'''sql
Select DISTINCT category as total_sale From retail_sales
'''
### COUNT of Rows
'''sql
Select 
Count(*)
From retail_sales
'''
### Cleaning of the data 
'''sql
SELECT * FROM retail_sales
Where 
     transactions_id IS NULL
     OR
     sale_date IS NULL
     OR
     sale_time IS NULL
     OR
     customer_id IS NULL 
     OR
     gender IS NULL
     OR 
     age IS NULL
     OR
     category IS NULL
     OR
     quantiy IS NULL
     OR
     price_per_unit IS NULL
     OR
     cogs IS NULL
     OR 
     total_sale IS NULL;
     '''
    
### Deleting the data where null is there
'''sql
     SET SQL_SAFE_UPDATES = 0;
     DELETE FROM retail_sales
     WHERE 
     transactions_id IS NULL
     OR
     sale_date IS NULL
     OR
     sale_time IS NULL
     OR
     customer_id IS NULL 
     OR
     gender IS NULL
     OR 
     age IS NULL
     OR
     category IS NULL
     OR
     quantiy IS NULL
     OR
     price_per_unit IS NULL
     OR
     cogs IS NULL
     OR 
     total_sale IS NULL;
'''
## Data Analysis & Business Key Problems & Answers 

### 1) Write a SQL query to retrieve all columns for sales made on '2022-11-05'
'''sql 
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';
'''
### 2) Q.2 write a SQL query to retrieve all transactions where category is 'clothing' and the quantity sold is more than 3 in the month of NOV-2022
'''sql
SELECT * FROM retail_sales
WHERE 
category = 'Clothing'
AND
DATE_FORMAT(sale_date, '%Y-%m')  = '2022-11'
AND 
quantiy >= 3;
'''
### 3) Write a SQL query to calculate the total sales (total sale) for each category and also get the total orders.
'''sql
SELECT 
      category, 
      SUM(total_sale) as net_sale,
      COUNT(*) as total_orders
FROM retail_sales
Group by 1
'''
### 4)  Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category. 
'''sql
SELECT 
ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE 
     category = 'Beauty' 
'''
### 5) Write a SQL query to find all transactions where the total_sale is greater than 1000.
'''sql
SELECT * FROM retail_sales
WHERE total_sale > 1000
'''
### 6) Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category 
'''sql

SELECT 
      category,
      gender,
      Count(*) as total_number_of_transaction 
 FROM retail_sales
 GROUP BY 1,2
 ORDER BY 1
 '''
 ### 7) Write a SQL query to calculate the average sale for each month. Find out best selling month in each year 
 '''sql
 SELECT * FROM 
 (
    SELECT 
	year(sale_date),
	month(sale_date),
	ROUND(AVG(total_sale), 2) as average_sale,
    RANK() OVER (PARTITION BY year(sale_date) order by avg(total_sale) DESC ) AS rank_
    FROM retail_sales
    GROUP BY 1,2
 ) as t1
 WHERE rank_ = '1'
'''



 ### 8) Write a SQL query to find the top 5 customers based on the highest total sales
 '''sql
 SELECT 
	   customer_id, 
	   SUM(total_sale) as total_sales
 FROM retail_sales
 GROUP BY 1
 ORDER BY 2 DESC 
 LIMIT 5
 '''

 
 ### 9) write a SQL query to find the number of unique customers who purchased items from each category
 '''sql
 SELECT 
       category,
       COUNT(Distinct(customer_id))
 FROM retail_sales
 GROUP by 1
 '''

### 10) Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening>17)
'''sql
WITH hourly_sale 
AS 
(
SELECT *,
      
      CASE
          WHEN HOUR(sale_time) < 12 THEN 'Morning'
          WHEN HOUR(sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
          ELSE 'Evening'
	  END as shift 
FROM retail_sales
)
SELECT 
      shift,
      count(*) as total_orders
FROM hourly_sale
GROUP BY 1 
'''



     
     







 
