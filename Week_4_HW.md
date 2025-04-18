# SQL Queries for Sales, Customers, and Items

---

## 1. Total number of orders completed on 18th March 2023

```sql
SELECT COUNT(*) AS total_orders
FROM SALES
WHERE Date = '2023-03-18';
```

---

## 2. Total number of orders completed on 18th March 2023 by customer 'John Doe'

```sql
SELECT COUNT(*) AS total_orders
FROM SALES S
JOIN CUSTOMERS C ON S.Customer_id = C.Customer_id
WHERE S.Date = '2023-03-18'
  AND C.First_name = 'John'
  AND C.Last_name = 'Doe';
```

---

## 3. Total number of customers that purchased in January 2023 and the average amount spent per customer

```sql
SELECT 
    COUNT(DISTINCT Customer_id) AS total_customers,
    AVG(customer_total_spent) AS avg_spent_per_customer
FROM (
    SELECT Customer_id, SUM(Revenue) AS customer_total_spent
    FROM SALES
    WHERE Date BETWEEN '2023-01-01' AND '2023-01-31'
    GROUP BY Customer_id
) AS customer_spending;
```

---

## 4. Departments that generated less than $600 in revenue in 2022

```sql
SELECT I.Department, SUM(S.Revenue) AS total_revenue
FROM SALES S
JOIN ITEMS I ON S.Item_id = I.Item_id
WHERE S.Date BETWEEN '2022-01-01' AND '2022-12-31'
GROUP BY I.Department
HAVING SUM(S.Revenue) < 600;
```

---

## 5. Most and least revenue generated by a single order

```sql
SELECT 
    MAX(Revenue) AS highest_order_revenue,
    MIN(Revenue) AS lowest_order_revenue
FROM SALES;
```

---

## 6. Orders that were purchased on our most lucrative day

```sql
WITH daily_revenue AS (
    SELECT Date, SUM(Revenue) AS total_revenue
    FROM SALES
    GROUP BY Date
),
max_day AS (
    SELECT Date
    FROM daily_revenue
    ORDER BY total_revenue DESC
    LIMIT 1
)
SELECT *
FROM SALES
WHERE Date = (SELECT Date FROM max_day);
```
