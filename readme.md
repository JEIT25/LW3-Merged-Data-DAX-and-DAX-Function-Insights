# Laboratory Work 3 - Merged Data, DAX, and DAX Function Insights

# Google Slide Link: https://docs.google.com/presentation/d/1rTHQT2aWJlmE1kSo4lpp4DFjC7Z5Xd2CP9-y_RO912s/edit?usp=sharing

## 🎯 Lab Objectives
* Transform merged flat datasets into a Star Schema.
* Identify and build Fact and Dimension tables.
* Create and validate Power BI relationships (One-to-Many).
* Author fundamental DAX measures and Time Intelligence functions.

---

## 🏗️ Data Modeling Fundamentals

### Part 1: Review the Merged Dataset
**1. Is the dataset in flat-table format?**
Yes. A flat table contains both descriptive attributes (customers/regions) and quantitative transactions (sales/orders) combined into a single, wide, highly repetitive table.

**2. Which columns describe customers?**
`CustomerID`, `CustomerName`, `Segment`, and `Country`/`Region`.

**3. Which columns represent transactions?**
`OrderID`, `OrderDate`, `SalesAmount`, `Quantity`, and `Product`.

### Part 2: Identify Fact and Dimension Tables
**1. Why must dimension tables contain unique keys?**
Dimension tables act as lookup tables. They must contain unique keys to identify each entity (like one specific customer) without ambiguity, which is required to form the "One" side of a One-to-Many relationship.

**2. What problems occur if duplicates exist in DimCustomer?**
It would trigger a Many-to-Many relationship, causing ambiguity in the data model. Filtering would become unpredictable, and DAX calculations would likely double-count or return incorrect aggregations.

### Part 3 & 4: Keys and Relationships
**1. What is a primary key?**
A unique identifier for every single record in a table (e.g., `CustomerID` in `DimCustomer`).

**2. Why is CustomerID a foreign key in FactSales?**
Because it references the Primary Key in the `DimCustomer` table. It acts as the bridge that links a specific sales transaction back to the customer who made it.

**3. Why should relationships flow from dimension to fact?**
Filter context flows downstream. When you select a Region in a visual (Dimension), the relationship line acts as a filter that flows down into the Fact table, calculating only the sales for that specific Region.

**4. What happens if the relationship is Many-to-Many?**
Standard filtering breaks down. Power BI cannot determine which table should filter the other, leading to inaccurate sub-totals and complex, computationally expensive cross-filtering requirements.

### Part 5: Validate the Data Model
**1. Did sales aggregate correctly per customer?**
Yes. Because of the active One-to-Many relationship, dropping `CustomerName` into a visual successfully sliced the `FactSales[SalesAmount]` by each specific individual.

**2. What would happen if relationships were missing?**
The visual would break. Every single customer row in the table would display the exact same Grand Total of all sales, rather than their individual contribution, because Power BI wouldn't know how to filter the math.

---

## 🧮 Introduction to DAX & Time Intelligence

### Fundamental DAX Questions
**1. What is a DAX measure?**
A DAX (Data Analysis Expressions) measure is a dynamic, portable calculation formula. Unlike static spreadsheet cells, measures recalculate instantly based on the filters/context of the visual they are placed in.

**2. Difference between SUM and AVERAGE?**
`SUM` adds all numeric values in a column together to find a grand total. `AVERAGE` adds all the numeric values together and then divides by the total count of those rows to find the typical transaction size.

**3. Why use measures instead of calculated columns?**
Calculated columns are evaluated row-by-row and take up physical RAM/memory in your dataset. Measures are calculated "on-the-fly" using the CPU. Using measures keeps the Power BI file size small and highly performant.

### Time Intelligence Questions
**1. What is time intelligence in DAX?**
It is a category of DAX functions designed to manipulate data contexts over dates (e.g., calculating Year-to-Date, Month-over-Month growth, or comparing current sales to the same period last year).

**2. Why is a date table (DimDate) required?**
Time Intelligence functions require a continuous, unbroken sequence of dates (every day of the year) to calculate accurately. Fact tables almost always have "gaps" (e.g., days where no sales occurred), which causes Time Intelligence functions to fail.

**3. How does PREVIOUSMONTH help analyze trends?**
It modifies the filter context to look at the exact same calculation, but shifted back by one month. This allows us to mathematically compare current performance against past performance to find growth/decline percentages.

**4. What insights can YTD Sales provide?**
Year-to-Date (YTD) provides a cumulative running total of performance from January 1st to the current date. It is the primary metric businesses use to track if they are pacing correctly to hit their annual operational targets.

---

## 🚀 Enhancement Activities Completed
* **Created DimDate:** Authored a continuous calendar table using `CALENDAR(DATE(2025,1,1), DATE(2025,12,31))` to enable Time Intelligence functions.
* **Schema Performance:** Transitioning from a flat table to a Star Schema vastly improved the logical structure of the data, reducing data redundancy (removing repeated customer names from the fact table) and optimizing the VertiPaq engine's compression.
