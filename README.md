# My-Projects-

### Project Tittle: Using SQL Server Environment to Analyze LITA Capstone Dataset

### Project Overview
This project involved analyzing sales and customer data for a retail store to assess performance and customer trends. Using MySQL, I evaluated total sales, product popularity, regional contributions, and monthly sales trends. Key findings included identifying the highest-selling products, top-spending customers, and regions contributing the most to overall sales. Additionally, I assessed customer subscription data to understand revenue by subscription type and the status of active subscriptions. The analysis provided insights into seasonal demand, customer loyalty, and product performance, enabling data-driven recommendations for maximizing revenue, optimizing inventory, and enhancing customer retention strategies for the retail store.

### Data Decsription 
The dataset comprised two tables: Orders and Customers. The Orders table included details such as OrderID, CustomerID, Product, Region, OrderDate, Quantity, and UnitPrice, providing information on sales transactions, product categories, and regional performance. Key metrics such as total sales, transaction count, and monthly revenue were derived from this data. The Customers table contained CustomerID, CustomerName, Region, SubscriptionType, SubscriptionStart, SubscriptionEnd, Canceled, and Revenue fields, giving insights into customer demographics, subscription status, and revenue per customer. Together, these datasets facilitated a comprehensive analysis of product performance, customer trends, and regional sales contributions for the retail store.

### Tool Used 
MySQL Workbench was essential for performing efficient data analysis on retail sales and customer data. Its SQL editor allowed for advanced querying, enabling detailed insights into sales performance, customer trends, and regional contributions. The tool’s intuitive interface, along with built-in data import and export functions, streamlined data manipulation and analysis.

### Data Loading and Preparation 
Data was loaded into MySQL Workbench by importing CSV files for both the Orders and Customers datasets. The data was structured into tables, with appropriate data types assigned to each column. Basic data cleaning, including date formatting and ensuring consistent data types, was conducted to optimize data accuracy for analysis.

### Data Analysis 
1) Total Sales by Product Category - Calculated total sales for each product, showing revenue contributions by category.

2) Sales Transactions by Region - Counted sales transactions in each region to identify high-activity areas.

3) Highest Selling Product - Identified the product with the highest total sales value.

4) Total Revenue by Product - Computed revenue generated by each product for financial insights.

5) Monthly Sales Totals for Current Year - Summed monthly sales for the current year, identifying seasonal trends.

6) Top Five Customers by Purchase Amount - Ranked customers by total purchase amount, focusing on high-value clients.

7) Sales Contribution Percentage by Region - Calculated each region’s sales percentage of total revenue.

8) Products with No Sales in the Last Quarter - Listed products without recent sales, highlighting potential underperformers.

9) Customer Basic Information - Retrieved key details on each customer for demographic analysis.

10) Revenue by Subscription Type - Summed revenue by subscription type, assessing customer preferences.

11) Active Subscriptions by Region - Counted active subscriptions by region, showing distribution of customer engagement.

12) Customers with Subscriptions Ending Last Month - Identified customers with recently ended subscriptions to assess retention needs.

### SQL Codes for the Analysis
-- Step 1: Create the Database
CREATE DATABASE retail_store;
USE retail_store;

-- Step 2: Create Orders Table
CREATE TABLE orders (
    OrderID INT PRIMARY KEY,
    CustomerId VARCHAR(50),
    Product VARCHAR(50),
    Region VARCHAR(50),
    OrderDate DATE,
    Quantity INT,
    UnitPrice DECIMAL(10, 2)
);

-- Step 3: Create Customers Table
CREATE TABLE customers (
    CustomerID INT PRIMARY KEY,
    CustomerName VARCHAR(50),
    Region VARCHAR(50),
    SubscriptionType VARCHAR(50),
    SubscriptionStart DATE,
    SubscriptionEnd DATE,
    Canceled BOOLEAN,
    Revenue DECIMAL(10, 2)
);

-- Step 4: Load Data from CSV Files
-- Note: You would typically use a tool to import the CSV files directly.
-- Here, I'm providing the SQL syntax for reference if you were to use `LOAD DATA INFILE`.

-- Load Orders Data (assuming file is accessible)
LOAD DATA INFILE '/path/to/orders.csv'
INTO TABLE orders
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"' 
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

-- Load Customers Data (assuming file is accessible)
LOAD DATA INFILE '/path/to/customers.csv'
INTO TABLE customers
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"' 
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

-- Step 5: Perform Analysis

-- 1. Total Sales by Product Category
SELECT Product, SUM(Quantity * UnitPrice) AS TotalSales
FROM orders
GROUP BY Product;

-- 2. Sales Transactions by Region
SELECT Region, COUNT(OrderID) AS NumberOfSales
FROM orders
GROUP BY Region;

-- 3. Highest Selling Product
SELECT Product, SUM(Quantity * UnitPrice) AS TotalSales
FROM orders
GROUP BY Product
ORDER BY TotalSales DESC
LIMIT 1;

-- 4. Total Revenue by Product
SELECT Product, SUM(Quantity * UnitPrice) AS TotalRevenue
FROM orders
GROUP BY Product;

-- 5. Monthly Sales Totals for Current Year
SELECT MONTH(OrderDate) AS Month, SUM(Quantity * UnitPrice) AS MonthlySales
FROM orders
WHERE YEAR(OrderDate) = YEAR(CURDATE())
GROUP BY MONTH(OrderDate);

-- 6. Top Five Customers by Total Purchase Amount
SELECT CustomerId, SUM(Quantity * UnitPrice) AS TotalPurchaseAmount
FROM orders
GROUP BY CustomerId
ORDER BY TotalPurchaseAmount DESC
LIMIT 5;

-- 7. Percentage of Total Sales by Region
SELECT Region, 
       SUM(Quantity * UnitPrice) AS RegionSales, 
       (SUM(Quantity * UnitPrice) / (SELECT SUM(Quantity * UnitPrice) FROM orders) * 100) AS PercentageOfTotalSales
FROM orders
GROUP BY Region;

-- 8. Products with No Sales in the Last Quarter
SELECT DISTINCT Product
FROM orders
WHERE Product NOT IN (
    SELECT Product
    FROM orders
    WHERE OrderDate >= DATE_SUB(CURDATE(), INTERVAL 3 MONTH)
);

-- 9. Basic Customer Information
SELECT CustomerID, CustomerName, Region, SubscriptionType, Revenue
FROM customers;

-- 10. Total Revenue by Subscription Type
SELECT SubscriptionType, SUM(Revenue) AS TotalRevenue
FROM customers
GROUP BY SubscriptionType;

-- 11. Active Subscriptions by Region
SELECT Region, COUNT(CustomerID) AS ActiveSubscriptions
FROM customers
WHERE Canceled = FALSE
GROUP BY Region;

-- 12. Customers with Subscriptions Ending Last Month
SELECT CustomerID, CustomerName
FROM customers
WHERE SubscriptionEnd BETWEEN DATE_SUB(CURDATE(), INTERVAL 1 MONTH) AND CURDATE();










