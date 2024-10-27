# SQL Queries Documentation

## Table of Contents
1. [Introduction](#introduction)
2. [Query Structure and Syntax](#query-structure-and-syntax)
3. [JOIN Operations](#join-operations)
4. [WHERE Clause Conditions and Operators](#where-clause-conditions-and-operators)
5. [Aggregation Functions](#aggregation-functions)
6. [GROUP BY and HAVING Clauses](#group-by-and-having-clauses)
7. [Subqueries and Common Table Expressions (CTEs)](#subqueries-and-ctes)
8. [Window Functions](#window-functions)
9. [Advanced SQL Features](#advanced-sql-features)
10. [Table Relationships and Keys](#table-relationships-and-keys)
11. [Query Optimization Techniques](#query-optimization-techniques)
12. [Best Practices](#best-practices)
13. [Troubleshooting Common Issues](#troubleshooting-common-issues)
14. [Glossary of Terms](#glossary-of-terms)

## Introduction
This document provides a detailed explanation of various SQL concepts used in queries, ranging from basic syntax to advanced features like window functions and query optimization techniques. The document is designed for both beginners and experienced developers, covering practical examples from the provided queries to illustrate the concepts and best practices for efficient SQL development.

---

## Query Structure and Syntax
### Overview
SQL queries retrieve and manipulate data using a specific structure:
- **SELECT**: Defines the columns to retrieve.
- **FROM**: Specifies the table(s) from which to extract data.
- **WHERE**: Filters rows based on specific conditions.
- **GROUP BY**: Groups rows sharing a common attribute to perform aggregations.
- **ORDER BY**: Sorts the result set.
- **LIMIT**: Restricts the number of returned rows.

### Example
```sql
SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(product_key) FROM Product);
```

### Syntax Variations
- SQL statements may use clauses like **JOIN**, **HAVING**, **UNION**, and **CASE** for data manipulation and filtering.

### Best Practices
- Use explicit column names instead of `SELECT *` for better performance and clarity.
- Align and format queries for readability.

### Common Pitfalls
- Using `SELECT *` may result in performance issues and unintended consequences when the table structure changes.

### Performance Considerations
- Index columns used in **WHERE**, **JOIN**, and **ORDER BY** clauses to optimize query performance.

---

## JOIN Operations
### Overview
**JOIN** operations combine rows from multiple tables based on related columns. Types of joins include:
- **INNER JOIN**: Retrieves rows matching in both tables.
- **LEFT JOIN**: Retrieves all rows from the left table and matching rows from the right, with NULLs if no match.
- **RIGHT JOIN**: Similar to **LEFT JOIN** but includes all rows from the right table.
- **FULL OUTER JOIN**: Returns rows when there's a match in either table.

### Examples
```sql
SELECT product_name, year, price
FROM Sales s
JOIN Product p ON p.product_id = s.product_id;
```

### When and Why to Use
- **JOIN** operations are necessary when accessing data spread across multiple tables connected by foreign keys.

### Syntax and Variations
- **INNER JOIN**: The default and most common type.
- **LEFT JOIN**: Used when all rows from one table are required, regardless of matches.

### Best Practices
- Index join columns for better performance.
- Always use explicit column names in joins to avoid ambiguity.

### Performance Considerations
- Large table joins can be resource-intensive; use indexes and consider filtering with **WHERE**.

---

## WHERE Clause Conditions and Operators
### Overview
The **WHERE** clause filters rows based on conditions, using operators like:
- **=, <, >, !=**: Standard comparison.
- **LIKE**: Pattern matching.
- **IN**: Checks for matches in a list.
- **BETWEEN**: Filters within a range.
- **IS NULL**: Checks for NULL values.

### Example
```sql
SELECT product_id
FROM products
WHERE low_fats = 'Y' AND recyclable = 'Y';
```

### Syntax Variations
- Combine multiple conditions using **AND** and **OR**.
- Use **REGEXP** for more complex pattern matching.

### Best Practices
- Use indexed columns in **WHERE** clauses for faster lookups.
- Avoid calculations in **WHERE** conditions.

### Performance Considerations
- Ensure columns frequently used in conditions are indexed.

---

## Aggregation Functions
### Overview
Aggregation functions compute summary statistics across rows:
- **SUM()**, **AVG()**, **COUNT()**, **MIN()**, **MAX()**.

### Example
```sql
SELECT user_id, COUNT(DISTINCT follower_id) AS followers_count
FROM Followers
GROUP BY user_id;
```

### Syntax and Variations
- Aggregations are used with **GROUP BY** to summarize data by groups.

### Best Practices
- Use aggregation functions to efficiently summarize and analyze large datasets.

### Performance Considerations
- Index columns used in **GROUP BY** for optimized performance.

---

## GROUP BY and HAVING Clauses
### Overview
- **GROUP BY** organizes rows sharing common properties into groups for aggregation.
- **HAVING** filters groups based on aggregate conditions, unlike **WHERE**.

### Example
```sql
SELECT class
FROM Courses C1
GROUP BY class
HAVING COUNT(DISTINCT student) >= 5;
```

### Syntax Variations
- Use **HAVING** to filter groups after aggregation.

### Best Practices
- Minimize the number of columns in **GROUP BY** to improve performance.

### Common Pitfalls
- Misuse of **WHERE** instead of **HAVING** with aggregate functions.

### Performance Considerations
- Indexing improves the efficiency of grouping and aggregation.

---

## Subqueries and Common Table Expressions (CTEs)
### Overview
- **Subqueries** are nested queries within another query.
- **CTEs** are named temporary result sets defined for clarity and reuse.

### Example
```sql
WITH all_ids AS (
    SELECT customer_id AS id
    FROM Customer
    UNION ALL
    SELECT referee_id AS id
    FROM Customer
)
SELECT id, COUNT(id) AS count_no_trans
FROM all_ids
GROUP BY id;
```

### When and Why to Use
- Use subqueries for dynamic filtering and CTEs for modular, readable queries.

### Syntax Variations
- CTEs begin with **WITH**, while subqueries are enclosed in parentheses.

### Best Practices
- Prefer CTEs for readability and maintainability when a subquery is reused.

### Performance Considerations
- Avoid deeply nested subqueries; they may impact performance.

---

## Window Functions
### Overview
Window functions calculate results over a set of table rows related to the current row, often used for ranking, cumulative sums, or averages.

### Example
```sql
SELECT
    contest_id,
    ROUND(COUNT(user_id) * 100 / (SELECT COUNT(user_id) FROM Users), 2) as percentage
FROM Register
GROUP BY contest_id
ORDER BY percentage DESC;
```

### Performance Considerations
- Partition and order by indexed columns for optimal performance.

---

## Advanced SQL Features
### Overview
Some advanced SQL features include:
- **CASE**: Conditional expressions within queries.
- **UNION**: Combines the result sets of two queries.
- **DISTINCT**: Ensures unique values in the result set.

### Example
```sql
SELECT name, population, area
FROM World
WHERE area >= 3000000 OR population >= 25000000;
```

### Performance Considerations
- Be mindful of performance impacts when using complex conditions.

---

## Table Relationships and Keys
### Overview
- **Primary Keys** uniquely identify rows.
- **Foreign Keys** establish relationships between tables.

### Best Practices
- Ensure proper indexing of primary and foreign keys for efficient joins.

---

## Query Optimization Techniques
### Overview
Optimizing SQL queries is crucial for performance:
- Index columns used in joins and where conditions.
- Avoid unnecessary subqueries.
- Prefer **EXISTS** over **IN** when dealing with large datasets.

### Example
```sql
SELECT tweet_id
FROM Tweets
WHERE LENGTH(content) > 15;
```

### Troubleshooting Common Issues
- Missing indexes can cause slow performance.
- Complex joins and aggregations without proper filtering can be resource-intensive.

---

## Best Practices
1. Use explicit column selection rather than `SELECT *`.
2. Index frequently used columns.
3. Break down complex queries into CTEs for clarity.
4. Always test queries for performance using **EXPLAIN**.

---

## Glossary of Terms
- **Aggregation**: Summarizes multiple rows.
- **CTE**: A named temporary result set.
- **Window Function**: A function that computes values across rows related to the current row.
- **Join**: Combines rows from multiple tables.
