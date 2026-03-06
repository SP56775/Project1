Retail Sales Analysis SQL Project
1. Project Overview

Project Title: Retail Sales Analysis
Level: Beginner
Database: p1_retail_db

This project demonstrates SQL skills used by data analysts to explore, clean, and analyze retail sales data. The project includes database creation, data cleaning, exploratory data analysis (EDA), and business analysis using SQL queries.

The goal of the project is to understand customer behavior, sales trends, and product performance from retail data.

2. Objectives

Database Setup

Create a retail sales database.

Store transaction data in structured format.

Data Cleaning

Detect and remove records containing missing or NULL values.

Exploratory Data Analysis (EDA)

Understand the dataset structure.

Identify number of customers, categories, and transactions.

Business Analysis

Answer real-world business questions using SQL queries.

3. Database Setup
Create Database
CREATE DATABASE p1_retail_db;
Create Table
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
Table Description
Column	Description
transactions_id	Unique ID for each transaction
sale_date	Date of sale
sale_time	Time of sale
customer_id	Unique customer ID
gender	Customer gender
age	Customer age
category	Product category
quantity	Quantity sold
price_per_unit	Price per unit
cogs	Cost of goods sold
total_sale	Total sale amount
4. Data Exploration & Cleaning
Total Number of Records
SELECT COUNT(*) FROM retail_sales;
Unique Customers
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
Unique Product Categories
SELECT DISTINCT category FROM retail_sales;
Check NULL Values
SELECT *
FROM retail_sales
WHERE
sale_date IS NULL
OR sale_time IS NULL
OR customer_id IS NULL
OR gender IS NULL
OR age IS NULL
OR category IS NULL
OR quantity IS NULL
OR price_per_unit IS NULL
OR cogs IS NULL;
Remove Records with Missing Data
DELETE FROM retail_sales
WHERE
sale_date IS NULL
OR sale_time IS NULL
OR customer_id IS NULL
OR gender IS NULL
OR age IS NULL
OR category IS NULL
OR quantity IS NULL
OR price_per_unit IS NULL
OR cogs IS NULL;
5. Data Analysis & Business Questions
1. Sales on a Specific Date

Retrieve all sales made on 5 November 2022

SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
2. Clothing Sales in November 2022 (Quantity > 4)
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
AND quantity >= 4;
3. Total Sales by Category
SELECT
category,
SUM(total_sale) AS net_sale,
COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
4. Average Age of Beauty Category Customers
SELECT
ROUND(AVG(age),2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
5. Transactions with Sales Greater Than 1000
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
6. Number of Transactions by Gender and Category
SELECT
category,
gender,
COUNT(*) AS total_transactions
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
7. Best Selling Month in Each Year
SELECT
year,
month,
avg_sale
FROM
(
SELECT
EXTRACT(YEAR FROM sale_date) AS year,
EXTRACT(MONTH FROM sale_date) AS month,
AVG(total_sale) AS avg_sale,
RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date)
ORDER BY AVG(total_sale) DESC) AS rank
FROM retail_sales
GROUP BY 1,2
) t1
WHERE rank = 1;
8. Top 5 Customers by Total Sales
SELECT
customer_id,
SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
9. Unique Customers per Category
SELECT
category,
COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
10. Orders by Sales Shift

Shift Definition:

Morning → Before 12 PM

Afternoon → 12 PM to 5 PM

Evening → After 5 PM

WITH hourly_sale AS
(
SELECT *,
CASE
WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
ELSE 'Evening'
END AS shift
FROM retail_sales
)

SELECT
shift,
COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
6. Key Findings
Customer Demographics

Customers belong to various age groups and purchase products across multiple categories like Clothing, Beauty, and others.

High Value Purchases

Some transactions have sales greater than 1000, indicating premium product purchases.

Sales Trends

Monthly sales analysis helps identify peak sales months for the business.

Customer Insights

The project identifies top spending customers and customer distribution across categories.

7. Reports Generated
Sales Summary

Overview of:

Total sales

Number of transactions

Category performance

Trend Analysis

Monthly sales trends

Best performing months

Customer Insights

Top customers

Unique customers per category

8. Conclusion

This project demonstrates how SQL can be used for data cleaning, analysis, and business insights. By analyzing retail sales data, businesses can better understand:

Customer purchasing patterns

Product performance

Sales trends over time

These insights can help businesses improve marketing strategies, inventory planning, and customer targeting.

9. How to Use the Project

Clone the repository from GitHub.

Run the database setup SQL script.

Import the sales dataset.

Execute the analysis queries.

Modify queries to explore additional insights.
