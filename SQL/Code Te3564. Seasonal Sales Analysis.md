## https://leetcode.com/problems/seasonal-sales-analysis/description/


## Problem Overview
You are given two tables:
- `sales`: Contains individual product sales (product_id, sale_date, quantity, price)
- `products`: Contains product info (product_id, product_name, category)

Your task is to **find the most popular product category for each season** (Winter, Spring, Summer, Fall).
- Popularity is based on **total quantity sold**.
- In case of a tie, choose the category with **higher revenue** (quantity × price).
- Return results ordered by season.

---

## Season Mapping
| Season | Months              |
|--------|---------------------|
| Winter | December, January, February (12, 1, 2) |
| Spring | March, April, May (3, 4, 5)     |
| Summer | June, July, August (6, 7, 8)    |
| Fall   | September, October, November (9, 10, 11) |

---
## Explanation (Simple)
1. First, we join `sales` and `products` into a common temp table.
2. For each season:
   - We filter rows by the season's months.
   - Group by category.
   - Calculate total quantity and revenue (`price * quantity`).
   - Order by quantity desc, then revenue desc (tie-breaker).
   - Pick the top 1 (most popular).
3. Combine results for all seasons using `UNION`.

---

## Output Columns
| season | category | total_quantity | total_revenue |
|--------|----------|----------------|----------------|

Each row represents the top category for that season.
---

## SQL Solution
```sql
WITH TEMP AS (
    SELECT 
        A.sale_id,
        A.product_id,
        A.sale_date,
        A.quantity,
        A.price,
        B.category
    FROM sales AS A
    LEFT JOIN products AS B ON A.product_id = B.product_id
),

WINTER AS (
    SELECT 
        'Winter' AS season,
        category,
        SUM(quantity) AS total_quantity,
        1.0 * SUM(price * quantity) AS total_revenue
    FROM TEMP
    WHERE EXTRACT(MONTH FROM sale_date) IN (1, 2, 12)
    GROUP BY category
    ORDER BY total_quantity DESC, total_revenue DESC
    LIMIT 1
),

SPRING AS (
    SELECT 
        'Spring' AS season,
        category,
        SUM(quantity) AS total_quantity,
        1.0 * SUM(price * quantity) AS total_revenue
    FROM TEMP
    WHERE EXTRACT(MONTH FROM sale_date) IN (3, 4, 5)
    GROUP BY category
    ORDER BY total_quantity DESC, total_revenue DESC
    LIMIT 1
),

SUMMER AS (
    SELECT 
        'Summer' AS season,
        category,
        SUM(quantity) AS total_quantity,
        1.0 * SUM(price * quantity) AS total_revenue
    FROM TEMP
    WHERE EXTRACT(MONTH FROM sale_date) IN (6, 7, 8)
    GROUP BY category
    ORDER BY total_quantity DESC, total_revenue DESC
    LIMIT 1
),

FALL AS (
    SELECT 
        'Fall' AS season,
        category,
        SUM(quantity) AS total_quantity,
        1.0 * SUM(price * quantity) AS total_revenue
    FROM TEMP
    WHERE EXTRACT(MONTH FROM sale_date) IN (9, 10, 11)
    GROUP BY category
    ORDER BY total_quantity DESC, total_revenue DESC
    LIMIT 1
)

SELECT * FROM FALL
UNION
SELECT * FROM SPRING
UNION
SELECT * FROM SUMMER
UNION
SELECT * FROM WINTER;
```

---

