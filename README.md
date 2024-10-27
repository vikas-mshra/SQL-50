# SQL Concepts and Use Cases

## Overview
This documentation provides a concise explanation of each SQL concept used in the provided queries. It serves as a quick reference to understand what each query does and when it can be applied.

---

## 1. **Basic Query Structure**

### Query Example:
```sql
SELECT customer_id
FROM Customer;
```

### Use Case:
- Retrieve data from a single table.
- Use **SELECT** and **FROM** to define what to retrieve and from where.

---

## 2. **JOIN Operations**

### Query Example:
```sql
SELECT product_name, year, price
FROM Sales s
JOIN Product p ON p.product_id = s.product_id;
```

### Use Case:
- Combine rows from multiple tables based on a related column (e.g., **INNER JOIN**, **LEFT JOIN**).
- Use when data is spread across multiple tables.

---

## 3. **WHERE Clause**

### Query Example:
```sql
SELECT product_id
FROM products
WHERE low_fats = 'Y' AND recyclable = 'Y';
```

### Use Case:
- Filter rows based on conditions.
- Commonly used for conditional selection and data filtering.

---

## 4. **GROUP BY**

### Query Example:
```sql
SELECT user_id, COUNT(DISTINCT follower_id) AS followers_count
FROM Followers
GROUP BY user_id;
```

### Use Case:
- Group rows based on a common attribute to perform aggregation (e.g., count followers per user).

---

## 5. **HAVING Clause**

### Query Example:
```sql
SELECT class
FROM Courses C1
GROUP BY class
HAVING COUNT(DISTINCT student) >= 5;
```

### Use Case:
- Filter groups created by **GROUP BY** based on aggregate conditions (e.g., classes with at least 5 students).

---

## 6. **Subqueries**

### Query Example:
```sql
SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(product_key) FROM Product);
```

### Use Case:
- Use a nested query within another query for dynamic filtering or value comparison.

---

## 7. **Common Table Expressions (CTEs)**

### Query Example:
```sql
WITH all_ids AS (
    SELECT requester_id AS id
    FROM RequestAccepted
    UNION ALL
    SELECT accepter_id AS id
    FROM RequestAccepted
)
SELECT id, COUNT(id) AS count_no_trans
FROM all_ids
GROUP BY id;
```

### Use Case:
- Define temporary result sets for modular, readable queries, especially useful when reusing the result multiple times.

---

## 8. **Aggregate Functions**

### Query Example:
```sql
SELECT user_id, COUNT(DISTINCT follower_id) AS followers_count
FROM Followers
GROUP BY user_id;
```

### Use Case:
- Summarize data using functions like **COUNT**, **SUM**, **AVG**, etc., often with **GROUP BY**.

---

## 9. **DISTINCT Keyword**

### Query Example:
```sql
SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id;
```

### Use Case:
- Retrieve unique values, removing duplicates from the result set.

---

## 10. **CASE Statement**

### Query Example:
```sql
SELECT
    product_id,
    CASE WHEN P1.change_date <= '2019-08-16' THEN new_price
    ELSE IFNULL(
        (SELECT new_price FROM products P2 
        WHERE P2.product_id = P1.product_id AND change_date <= '2019-08-16' 
        ORDER BY change_date DESC LIMIT 1), 10)
    END AS price
FROM Products P1;
```

### Use Case:
- Apply conditional logic directly within SQL queries.

---

## 11. **UNION and UNION ALL**

### Query Example:
```sql
(SELECT name FROM Users u LEFT JOIN MovieRating m ON u.user_id = m.user_id GROUP BY name)
UNION ALL
(SELECT title FROM MovieRating mr LEFT JOIN Movies m ON mr.movie_id = m.movie_id WHERE LEFT(created_at, 7) = '2020-02');
```

### Use Case:
- Combine results from multiple queries into a single result set.
- **UNION** removes duplicates; **UNION ALL** retains all rows.

---

## 12. **ORDER BY Clause**

### Query Example:
```sql
SELECT name
FROM Customer
WHERE referee_id IS NULL OR referee_id <> 2
ORDER BY name;
```

### Use Case:
- Sort the result set by one or more columns.

---

## 13. **LIMIT Clause**

### Query Example:
```sql
SELECT id, COUNT(id) AS num
FROM all_ids
GROUP BY id
ORDER BY num DESC
LIMIT 1;
```

### Use Case:
- Restrict the number of rows returned in the result set.

---

## 14. **Window Functions**

### Query Example:
```sql
SELECT
    ROUND(
        COUNT(A1.player_id) / (SELECT COUNT(DISTINCT A3.player_id) FROM Activity A3), 2
    ) AS fraction
FROM Activity A1;
```

### Use Case:
- Perform calculations across a set of rows related to the current row (e.g., ranking, cumulative totals).

---

## 15. **Self-Joins**

### Query Example:
```sql
SELECT w1.id
FROM Weather w1 LEFT JOIN Weather w2 ON DATEDIFF(w1.recordDate, w2.recordDate) = 1
WHERE w1.temperature > w2.temperature;
```

### Use Case:
- Join a table with itself to compare or relate rows within the same table.

---

## 16. **Cross Joins**

### Query Example:
```sql
SELECT s.student_id, s.student_name, sub.subject_name, IFNULL(grouped.attended_exams, 0) AS attended_exams
FROM Students s
CROSS JOIN Subjects sub;
```

### Use Case:
- Combine each row from one table with every row of another (useful for generating combinations).

---

## 17. **LEFT JOIN with NULL Handling**

### Query Example:
```sql
SELECT e.name, b.bonus
FROM Employee e LEFT JOIN Bonus b ON e.empId = b.empId
WHERE b.bonus IS NULL OR b.bonus < 1000;
```

### Use Case:
- Retrieve all rows from the left table and matching rows from the right; filter based on missing data or other conditions.

---

## 18. **Pattern Matching (LIKE)**

### Query Example:
```sql
SELECT patient_id, patient_name
FROM Patients
WHERE conditions LIKE 'DIAB1%' OR conditions LIKE '% DIAB1%';
```

### Use Case:
- Search for patterns within text data using **LIKE** and wildcard characters (`%`).

---

## 19. **CASE with Aggregates**

### Query Example:
```sql
SELECT
    machine_id,
    ROUND(AVG(CASE WHEN activity_type = 'start' THEN -timestamp ELSE timestamp END) * 2, 3) AS processing_time
FROM Activity
GROUP BY machine_id;
```

### Use Case:
- Apply conditional logic within aggregate functions to calculate different results based on specific criteria.

---

## 20. **Using EXISTS**

### Query Example:
```sql
SELECT customer_id
FROM Customer
WHERE EXISTS (
    SELECT 1
    FROM Orders o
    WHERE o.customer_id = Customer.customer_id
);
```

### Use Case:
- Check for the existence of rows that satisfy certain criteria in a subquery.

---

## 21. **Date Functions**

### Query Example:
```sql
SELECT activity_date AS day, COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE activity_date <= '2019-07-27' AND DATEDIFF('2019-07-27', activity_date) <= 30
GROUP BY day;
```

### Use Case:
- Manipulate and filter dates using functions like **DATEDIFF**.

---

## 22. **IN and NOT IN**

### Query Example:
```sql
SELECT customer_id
FROM Visits
WHERE visit_id NOT IN (SELECT visit_id FROM Transactions);
```

### Use Case:
- Filter rows based on whether values are present or absent in a subquery result.

---
