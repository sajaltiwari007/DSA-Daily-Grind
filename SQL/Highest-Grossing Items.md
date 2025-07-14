Ques Link-: https://datalemur.com/questions/sql-highest-grossing


###  Problem Statement

You're given a table `product_spend` that records Amazon customer transactions across different product categories. Your task is to identify the **top two highest-grossing products** within **each category** based on the **total spend in the year 2022**.

---

### Table Structure

**Table Name:** `product_spend`

| Column Name       | Type      |
|-------------------|-----------|
| category          | string    |
| product           | string    |
| user_id           | integer   |
| spend             | decimal   |
| transaction_date  | timestamp |

---

###  Example Input

| category    | product          | user_id | spend  | transaction_date       |
|-------------|------------------|---------|--------|------------------------|
| appliance   | refrigerator      | 165     | 246.00 | 2021-12-26 12:00:00    |
| appliance   | refrigerator      | 123     | 299.99 | 2022-03-02 12:00:00    |
| appliance   | washing machine   | 123     | 219.80 | 2022-03-02 12:00:00    |
| electronics | vacuum            | 178     | 152.00 | 2022-04-05 12:00:00    |
| electronics | wireless headset  | 156     | 249.90 | 2022-07-08 12:00:00    |
| electronics | vacuum            | 145     | 189.00 | 2022-07-15 12:00:00    |

---

###  Expected Output

| category    | product          | total_spend |
|-------------|------------------|-------------|
| appliance   | refrigerator      | 299.99      |
| appliance   | washing machine   | 219.80      |
| electronics | vacuum            | 341.00      |
| electronics | wireless headset  | 249.90      |

---

### Approach

1. **Filter transactions** for the year **2022**.
2. **Group by** `category` and `product`, then **sum** the `spend`.
3. Use a **window function** (`ROW_NUMBER()`) to rank products **within each category** by their total spend.
4. Select only the **top 2** products (rank ≤ 2) per category.

---

###  SQL Query Solution

```sql
WITH product_totals AS (
    SELECT 
        category, 
        product, 
        SUM(spend) AS money
    FROM product_spend
    WHERE EXTRACT(YEAR FROM transaction_date) = 2022
    GROUP BY category, product
),
ranked AS (
    SELECT 
        category, 
        product, 
        money,
        ROW_NUMBER() OVER (PARTITION BY category ORDER BY money DESC) AS rn
    FROM product_totals
)
SELECT 
    category, 
    product, 
    money AS total_spend
FROM ranked
WHERE rn <= 2
ORDER BY category, total_spend DESC;
