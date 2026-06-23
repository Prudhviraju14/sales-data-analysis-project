# Sales Data Analysis Project

## Project Overview

This project analyzes retail electronics sales data sourced from Kaggle using Excel and SQL to identify revenue trends, top-performing products, top-performing cities, customer purchasing patterns, and business opportunities.

The project covers the complete data analysis workflow:

* Data Cleaning
* Duplicate Detection and Removal
* Feature Engineering
* Exploratory Data Analysis (EDA)
* KPI Development
* Interactive Dashboard Creation
* SQL-Based Business Analysis
* Business Insight Generation

The analysis was performed on 185,328 sales records and produced insights into revenue performance across products, cities, months, days, and customer purchasing hours.

## Tools Used

* Microsoft Excel
* Pivot Tables
* Pivot Charts
* Dashboard Design
* MySQL Workbench
* SQL(Data Import, Database Management)

  
## Project Objectives

The objective of this project was to:

1. Clean and prepare raw sales data for analysis.
2. Identify top-performing products and cities.
3. Analyze monthly and daily revenue trends.
4. Understand customer purchasing behavior.
5. Build a business-focused dashboard.
6. Validate findings using SQL queries.
7. Generate actionable business insights.

## Dataset Information
- **Source:** Kaggle (Public Dataset)
- **Total Rows:** 185,951
- **Time Period:** January 2019 — December 2020 (2 years)
- **Original Columns (11):** Order ID, Product, Quantity Ordered, Price Each, Sales, Order Date, Purchase Address, City, Hour, Month, Year
- **Helper Columns Added (5):** Date Only, Year Only, Month Only, Quarter Only, Day Only
- **Purpose of Helper Columns:** The Order Date column contained full datetime values. To enable monthly, quarterly, and day-wise analysis, five helper columns were extracted using Excel functions — YEAR(), MONTH(), TEXT(), and ROUNDUP()

## Project Phases

**Phase 1 — Dataset Understanding**
Imported the raw dataset containing 185,951 rows and 11 columns. Explored all columns, checked for missing values, identified duplicate records, and created a Data Dictionary sheet to document column names, data types, and observations.

**Phase 2 — Data Cleaning**
Used Conditional Formatting to visually highlight duplicate records. Created a Duplicate_Check column using the COUNTIFS formula to programmatically identify duplicates. Filtered and deleted duplicate rows, retaining only unique records in a cleaned dataset. Verified Sales calculation: Sales = Quantity Ordered × Price Each.

**Phase 3 — Feature Engineering**
Extracted five new helper columns from the Order Date column — Date Only, Year Only, Month Only, Quarter Only, and Day Only — using Excel functions INT(), YEAR(), TEXT(), and ROUNDUP(). These columns enabled time-based analysis that was not possible with the original datetime format.

**Phase 4 — Exploratory Data Analysis**
Built Pivot Tables to answer six business questions covering monthly revenue trends, city-wise performance, product revenue, units sold, peak order hours, and day-wise revenue. Each question was documented with its result and business insight.

**Phase 5 — Dashboard Creation**
Designed an Excel dashboard combining four KPI cards (Total Revenue, Total Orders, Top City, Top Month) and three bar charts (Monthly Revenue, City Revenue, Product Revenue) to present findings in a single, readable view.

## Dashboard

![Dashboard](dashboard.png)

---

## Business Questions and Answers

### Question 1 — Which month generated the highest revenue?
**Query:**
```sql
SELECT MONTHNAME(Order_Date), SUM(Sales)
FROM sales_data
GROUP BY MONTHNAME(Order_Date)
ORDER BY SUM(Sales) DESC;
```
**Result:** December — $4,603,020.84

**Insight:** December generated the highest revenue driven by holiday season shopping and festive demand. The business should increase inventory and run targeted promotions before December to maximize peak season revenue.

---

### Question 2 — Which city generated the highest revenue?
**Query:**
```sql
SELECT City, SUM(Sales) AS City_sales
FROM sales_data
GROUP BY City
ORDER BY SUM(Sales) DESC
LIMIT 3;
```
**Result:**
1. San Francisco — $8,247,060.32
2. Los Angeles — $5,443,980.13
3. New York City — $4,659,246.09

**Insight:** San Francisco generated the highest city revenue at $8.24M. The business should prioritize marketing spend and inventory allocation in San Francisco while exploring growth opportunities in lower-performing cities.

---

### Question 3 — Which product generated the highest revenue?
**Query:**
```sql
SELECT Product, SUM(Sales) AS Product_sales
FROM sales_data
GROUP BY Product
ORDER BY SUM(Sales) DESC
LIMIT 3;
```
**Result:**
1. MacBook Pro Laptop — $8,027,400.00
2. iPhone — $4,791,500.00
3. ThinkPad Laptop — $4,125,958.74

**Insight:** MacBook Pro Laptop generated the highest product revenue at $8.02M despite selling fewer units. High-priced products drive the most revenue even when sold in lower quantities. The business should maintain premium product offerings as primary revenue drivers.

---

### Question 4 — Which product sold the most units?
**Query:**
```sql
SELECT Product, SUM(Quantity_Ordered) AS Total_units
FROM sales_data
GROUP BY Product
ORDER BY SUM(Quantity_Ordered) DESC
LIMIT 3;
```
**Result:**
1. AAA Batteries (4-pack) — 30,880 units
2. AA Batteries (4-pack) — 27,541 units
3. USB-C Charging Cable — 23,873 units

**Insight:** Low-cost accessories sold the highest number of units. This reveals a price-volume trade-off — high-priced products drive revenue while low-cost products drive volume. The business should ensure consistent stock availability for high-volume accessories throughout the year.

---

### Question 5 — At what hour do customers place the most orders?
**Query:**
```sql
SELECT Hour, COUNT(DISTINCT Order_ID) AS number_of_orders
FROM sales_data
GROUP BY Hour
ORDER BY COUNT(DISTINCT Order_ID) DESC
LIMIT 3;
```
**Result:**
1. Hour 19 (7 PM) — 12,354 orders
2. Hour 12 (12 PM) — 12,218 orders
3. Hour 11 (11 AM) — 12,142 orders

**Insight:** Customer order activity peaks during lunch breaks (12 PM) and post-work hours (7 PM). The business should schedule promotional notifications, discount offers, and email campaigns during these windows to maximize customer engagement and conversion rates.

**Note — Excel vs SQL Discrepancy:** Excel's Pivot Table returned 12,859 orders for Hour 19, while SQL returned 12,354. Excel's standard COUNT includes every row including repeated Order_IDs from multi-product orders, while SQL's COUNT(DISTINCT Order_ID) counts each unique order only once. The SQL result is more accurate.

---

### Question 6 — Which day generated the highest revenue?
**Query:**
```sql
SELECT DAYNAME(Order_Date) AS day_name, SUM(Sales) AS total_sales
FROM sales_data
GROUP BY DAYNAME(Order_Date)
ORDER BY SUM(Sales) DESC
LIMIT 3;
```
**Result:**
1. Tuesday — $5,084,397.91
2. Wednesday — $4,984,571.18
3. Sunday — $4,922,178.44

**Insight:** Tuesday generated the highest day-wise revenue at $5.08M. Revenue remains consistently strong throughout the week with no significant low-performing days. The business should ensure sufficient inventory and timely delivery capacity especially on weekdays.

---

## Key Insights and Recommendations

1. **Seasonal Revenue Peak:** December generated the highest monthly revenue of $4.6M driven by holiday season demand. The business should increase inventory in November and launch targeted campaigns before December.

2. **Geographic Market Strength:** San Francisco generated the highest city revenue at $8.24M, significantly ahead of other cities. The business should prioritize marketing and inventory allocation in San Francisco while investigating growth opportunities in underperforming cities.

3. **Price vs Volume Trade-off:** High-priced products like MacBook Pro Laptop ($8.02M revenue) generated the most revenue despite selling fewer units, while low-cost products like AAA Batteries sold the most units (30,880) but generated minimal revenue. A balanced product strategy should be maintained.

4. **Peak Purchase Hours:** Customer order activity peaks at 12 PM and 7 PM. Promotional notifications, discount offers, and email campaigns should be delivered during these windows to maximize conversion rates.

---

## Challenges and Solutions

**Challenge 1 — Identifying True Duplicates**
Initial duplicate detection using Conditional Formatting revealed that some Order IDs appeared multiple times. On closer inspection, not all repeated Order IDs were true duplicates — some had different products under the same Order ID while others were exact duplicates.

**Solution:** Created a Duplicate_Check column using COUNTIFS to flag rows where Order ID, Product, Price, and Address all matched exactly. Filtered and deleted only true duplicates while preserving legitimate multi-item orders.

**Challenge 2 — Choosing the Right Chart Type**
With no prior experience selecting chart types for business dashboards, initial attempts used pie charts and column charts which proved unreadable with many categories and large revenue values.

**Solution:** Tested pie, column, and bar charts side by side. Bar charts proved most effective — category names remained fully readable and value labels did not overlap.

---

## Limitations
- Dataset covers only 2019–2020 (2 years), limiting longer-term trend analysis
- Only a limited set of cities are included — findings cannot be generalized to represent nationwide sales patterns
- Expanding the dataset to include more cities and years would allow more comprehensive analysis

---

## Future Scope
- Recreate the complete analysis using advanced SQL (window functions, CTEs, stored procedures)
- Expand the dataset to include additional years and cities
- Build an interactive Tableau dashboard for more advanced visualization
- Apply Python for deeper statistical analysis and predictive modeling

---

## Conclusion
This project analyzed retail sales data through a complete workflow — from raw data exploration and cleaning, to feature engineering, business analysis, and dashboard visualization. Duplicate records were carefully identified and removed using a verification-based approach. Pivot Tables were used to answer six key business questions covering revenue trends, city performance, product performance, and customer purchase behavior. The findings were validated using SQL queries and consolidated into an interactive Excel dashboard highlighting the most important KPIs and top rankings.

---

## SQL Analysis

### Database Setup
- **Database:** sales_project
- **Table:** sales_data

```sql
CREATE TABLE sales_data(
    ID INT AUTO_INCREMENT PRIMARY KEY,
    Order_ID INT,
    Product VARCHAR(50),
    Quantity_Ordered INT,
    Price_Each DECIMAL(10,2),
    Order_Date DATETIME,
    Purchase_Address VARCHAR(100),
    Sales DECIMAL(10,2),
    City VARCHAR(50),
    Hour INT
);
```

### Data Import
Used LOAD DATA INFILE for bulk import instead of the GUI wizard which was too slow for 185,000+ rows.

```sql
LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 9.6/Uploads/sales_data_cleaned.csv'
INTO TABLE sales_data
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(@dummy1, Order_ID, Product, Quantity_Ordered, Price_Each, Order_Date,
@dummy2, @dummy3, @dummy4, @dummy5, Purchase_Address, @dummy6, @dummy7,
Sales, City, Hour);
```

### Excel vs SQL Comparison

| Feature | Excel | SQL |
|---|---|---|
| Data Import | Manual UI-based import | Bulk load via LOAD DATA INFILE |
| Analysis Method | Pivot Tables (drag and drop) | SELECT, GROUP BY, SUM, COUNT queries |
| Visualization | Built-in charts and dashboards | Requires external tools |
| Accuracy | Standard COUNT over-counts orders | COUNT(DISTINCT) counts unique orders accurately |
| Scalability | Slow with large datasets | Handles large datasets efficiently |
| Ease of Use | Beginner friendly, visual | Requires query writing, more precise control





























