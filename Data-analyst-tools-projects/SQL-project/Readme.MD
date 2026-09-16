# Food Order Database Analysis

## Problem Statement
Designed and analyzed a relational database for a food ordering system to uncover customer loyalty patterns, restaurant performance, and order trends over time.

## Tools Used
- MySQL / MySQL Workbench

## Database Schema
Designed 4 related tables with primary and foreign key constraints:

| Table | Columns |
|-------|---------|
| **Customers** | CustomerID (PK), CustomerName |
| **Restaurants** | RestaurantID (PK), RestaurantName, Cuisine |
| **Orders** | OrderID (PK), CustomerID (FK), RestaurantID (FK), OrderDate, Amount |
| **Ratings** | RatingID (PK), CustomerID (FK), RestaurantID (FK), RatingScore |

```sql
CREATE TABLE Customers(CustomerID int primary key, CustomerName varchar(100));

CREATE TABLE Restaurants(RestaurantID int primary key, RestaurantName varchar(100), cuisine varchar(50));

CREATE TABLE Orders(OrderID int primary key, CustomerID int, RestaurantID int, OrderDate datetime, Amount decimal(10,2),
    foreign key (CustomerID) references Customers(CustomerID),
    foreign key (RestaurantID) references Restaurants(RestaurantID));

CREATE TABLE Ratings(RatingID int primary key, CustomerID int, RestaurantID int, RatingScore int,
    foreign key (CustomerID) references Customers(CustomerID),
    foreign key (RestaurantID) references Restaurants(RestaurantID));
```

## Approach
1. **Designed the schema** with proper primary/foreign key relationships across 4 tables.
2. **Imported and validated the data** — confirmed row counts after import (150 customers, 20 restaurants, 2,997 orders, 1,238 ratings).
3. **Data cleaning checks**:
   - Checked for missing values across all tables — none found.
   - Checked for duplicate primary keys (OrderID, RatingID) — none found.
   - Checked for orphaned/inconsistent records (orders referencing non-existent customers or restaurants) — none found, confirming referential integrity.
   - Checked min/max values for order amount and rating score to spot outliers.
4. **Exploratory analysis** — examined unique cuisine types, customer loyalty patterns, and order timing trends.
5. **Advanced analysis** — used window functions (`RANK()`, `DENSE_RANK()`) to rank customers by loyalty.

## Key Queries & Insights

**1. Data validation — row counts after import**
```sql
SELECT COUNT(*) FROM Customers;   -- 150
SELECT COUNT(*) FROM Orders;      -- 2997
SELECT COUNT(*) FROM Ratings;     -- 1238
SELECT COUNT(*) FROM Restaurants; -- 20
```

**2. Data quality checks — duplicates, missing values, inconsistent records**
```sql
-- Check for missing values
SELECT COUNT(*) AS missing_value FROM Customers WHERE CustomerName IS NULL;

-- Check for duplicate primary keys
SELECT OrderID, COUNT(*) FROM Orders GROUP BY OrderID HAVING COUNT(*) > 1;

-- Check for orphaned records (orders with no matching customer)
SELECT o.OrderID, o.CustomerID FROM Orders o
LEFT JOIN Customers c ON o.CustomerID = c.CustomerID
WHERE c.CustomerID IS NULL;
```
✅ Result: No missing values, no duplicate keys, no orphaned records — clean, well-linked dataset.

**3. Customer loyalty ranking (using window functions)**
```sql
SELECT c.CustomerName, COUNT(o.OrderID) AS TotalOrders,
    RANK() OVER (ORDER BY COUNT(o.OrderID) DESC) AS LoyaltyRanking,
    DENSE_RANK() OVER (ORDER BY COUNT(o.OrderID) DESC) AS LoyaltyDenseRanking
FROM Orders o
JOIN Customers c ON o.CustomerID = c.CustomerID
GROUP BY c.CustomerName
ORDER BY TotalOrders DESC
LIMIT 20;
```
🏆 **Top customer:** Rajesh Menon — 95 orders (well ahead of the next-highest, Vikram Malhotra at 65).

**4. Top restaurants by orders, revenue, and rating**
```sql
-- Top restaurants by total orders
SELECT r.RestaurantName, COUNT(o.OrderID) AS TotalOrders
FROM Orders o JOIN Restaurants r ON o.RestaurantID = r.RestaurantID
GROUP BY r.RestaurantName ORDER BY TotalOrders DESC LIMIT 10;

-- Top restaurants by total revenue
SELECT r.RestaurantName, ROUND(SUM(o.Amount),2) AS TotalRevenue
FROM Orders o JOIN Restaurants r ON o.RestaurantID = r.RestaurantID
GROUP BY r.RestaurantName ORDER BY TotalRevenue DESC LIMIT 10;

-- Average rating per restaurant
SELECT r.RestaurantName, ROUND(AVG(rt.RatingScore),2) AS AvgRating, COUNT(rt.RatingID) AS TotalRatings
FROM Ratings rt JOIN Restaurants r ON rt.RestaurantID = r.RestaurantID
GROUP BY r.RestaurantName ORDER BY AvgRating DESC;
```
🏆 **Top restaurant by orders and revenue:** Taco Fiesta — 183 orders, ₹1,54,539.40 in revenue (clear leader on both metrics).
⭐ **Highest-rated restaurant:** Sweet Tooth — 4.41 average rating (51 ratings) — showing that popularity and rating quality don't always align to the same restaurant.

**5. Order trends — peak hours, days, and monthly patterns**
```sql
-- Peak order hours
SELECT HOUR(OrderDate) AS OrderHour, COUNT(*) AS NumOrders
FROM Orders GROUP BY OrderHour ORDER BY NumOrders DESC;

-- Peak order days (day of week)
SELECT DAYNAME(OrderDate) AS DayOfWeek, COUNT(*) AS NumOrders
FROM Orders GROUP BY DayOfWeek ORDER BY NumOrders DESC;

-- Monthly order trends
SELECT DATE_FORMAT(OrderDate,"%y-%m") AS Month, COUNT(*) AS NumOrders, ROUND(SUM(Amount),2) AS Revenue
FROM Orders GROUP BY Month;
```
🕕 **Peak order hour:** 6 PM (18:00) — 271 orders, followed by 12 PM (noon) with 260 orders — classic dinner and lunch rush pattern.
📅 **Peak order day:** Saturday (471 orders) — Sunday is the slowest (382 orders).
📈 **Peak revenue month:** February 2024 — ₹1,08,324.07 across 135 orders.

## Files
- `foodorderdb_queries.sql` — Full SQL script (schema creation, data cleaning, and analysis queries)

## Key Takeaways
- The database is clean and well-structured — zero missing values, zero duplicates, and full referential integrity between tables.
- A small group of highly loyal customers (led by Rajesh Menon at 95 orders) drives a disproportionate share of order volume.
- **Taco Fiesta** leads on both order volume and revenue, but **Sweet Tooth** has the highest customer satisfaction (4.41 rating) — a reminder that scale and quality are separate metrics worth tracking independently.
- Orders peak around dinner time (6 PM) and lunch (12 PM), and Saturday is the busiest day overall — useful for staffing and inventory planning.
- Revenue and order volume don't move perfectly together month to month — worth further investigation into average order value by month.
