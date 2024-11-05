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
```SQL
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
```

### Sample Visualizations 
![image](https://github.com/user-attachments/assets/9b845639-e8b2-4e28-8f44-0d5638e88bd2)


### Project Tittle: Excel Analysis
Project Overview
This project involved an in-depth analysis of customer and sales data for a retail store using Microsoft Excel. The primary goal was to identify customer segments, purchasing behaviors, and sales performance metrics to enhance marketing strategies and optimize inventory management. By utilizing Excel's powerful data manipulation tools, including pivot tables, formulas, and visualizations, actionable insights were derived that could inform business decisions and drive growth.

### Customer Data Analysis
Introduction
The customer data analysis report delves into understanding purchasing behaviors, spending patterns, and demographic insights. The objective was to segment customers, calculate key performance indicators (KPIs) like Customer Lifetime Value (CLV) and Average Order Value (AOV), and highlight factors influencing retention and engagement. These insights are crucial for optimizing marketing strategies and enhancing customer loyalty.

### Customer Segmentation Analysis
Customer segmentation was performed based on spending and purchasing patterns. The Average Order Value (AOV) was calculated by dividing each customer's total purchase amount by their total number of transactions. This metric provides insights into typical spending per order, helping to identify high-value and low-value customers.

Additionally, Customer Lifetime Value (CLV) was estimated by multiplying AOV by purchase frequency. This figure indicates the potential revenue a customer could generate over their lifetime, allowing for targeted upselling and retention strategies for high-value customers.

### Purchase Frequency and Recency Analysis
To understand customer loyalty, purchase frequency was calculated as the number of orders per customer. High-frequency customers were identified as those with multiple orders, suggesting strong loyalty, while low-frequency customers indicated potential targets for re-engagement strategies.

Recency analysis was conducted using purchase dates, categorizing customers based on how recently they made a purchase. Customers who hadn’t purchased in the last 6-12 months were identified as at risk of churn, signaling opportunities for targeted retention marketing.

### Demographic Breakdown
If demographic data such as age, gender, and location were available, these aspects were analyzed to gain insights into customer profiles. Segmenting customers by age groups revealed spending trends across demographics, while gender-based analysis highlighted differences in purchasing behavior. Regional analysis identified which areas contributed most to sales, guiding geographical focus for marketing efforts.

### Cohort Analysis
Cohort analysis grouped customers by their first purchase month to understand purchasing behavior over time. Tracking monthly spending and retention rates for each cohort allowed for the identification of trends in customer engagement and the effectiveness of marketing initiatives. This analysis provided insights into customer longevity and engagement post-acquisition.

### RFM Analysis
To prioritize customers based on overall value, an RFM (Recency, Frequency, Monetary) analysis was conducted. Customers were scored based on recency of their last purchase, total number of purchases, and revenue generated. This segmentation strategy enabled targeted marketing strategies for different customer groups, enhancing retention and engagement.

### Key Metrics and Visualization
Visualizations were created using charts and conditional formatting to highlight key insights. Bar charts displayed AOV, CLV, and total spending per segment, while conditional formatting highlighted high-priority customer segments, making it easy to identify areas for focused marketing efforts.

### Sales Performance Analysis
### Introduction
The sales performance analysis report examined the sales data of the retail store, focusing on top-selling products, regional performance differences, and monthly trends. The goal was to provide actionable insights that inform inventory management, marketing strategies, and regional expansions.

### Product Sales Analysis
A pivot table was created to assess individual product performance, showing total sales per product. This highlighted best-selling products and those contributing less to revenue. The average sales per product were calculated using the AVERAGEIF function, providing insight into product popularity and profitability.

### Regional Sales Performance
The analysis revealed significant variations in sales performance across regions. A pivot table displayed total sales by region, identifying high-revenue-generating areas and those needing attention. The total revenue by region was calculated using the SUMIF function, offering a clear breakdown of regional contributions.

### Monthly Sales Trends
A pivot table displayed monthly sales totals, providing insights into seasonal and monthly trends. This analysis highlighted peak sales months influenced by seasonal demand or promotions, aiding in inventory and staffing decisions to maximize efficiency.

### Customer Purchase Analysis
By analyzing customer purchases, the top five customers based on total purchase amounts were identified. This highlighted the company’s most loyal customers, presenting opportunities for loyalty programs or exclusive offers to enhance customer relationships.

### Identification of Low-Performing Products
The analysis identified products with no sales activity in the last quarter, allowing the company to consider clearance sales or discontinuation. Understanding low-performing products aids in optimizing inventory levels and reallocating resources to more popular items.

### Conclusion
The comprehensive Excel analyses of customer and sales data provide valuable insights into purchasing behavior, customer segmentation, and sales performance. By leveraging these insights, the retail store can enhance its marketing strategies, optimize inventory management, and foster customer loyalty. Ongoing analysis and updates are essential to adapt to changing market conditions and ensure sustained growth.

### Sample Visualizations for the Excel Analysis 












