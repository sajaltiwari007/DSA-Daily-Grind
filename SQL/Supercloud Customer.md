## Ques Link-: https://datalemur.com/questions/supercloud-customer

---

###  Problem Description

A Microsoft Azure Supercloud customer is defined as a customer who has purchased at least one product from **every product category** listed in the `products` table. Write a SQL query to return the IDs of all such customers.

---

###  Tables

#### `customer_contracts`

| customer\_id | product\_id | amount |
| ------------ | ----------- | ------ |
| 1            | 1           | 1000   |
| 1            | 3           | 2000   |
| 1            | 5           | 1500   |
| 2            | 2           | 3000   |
| 2            | 6           | 2000   |

#### `products`

| product\_id | product\_category | product\_name            |
| ----------- | ----------------- | ------------------------ |
| 1           | Analytics         | Azure Databricks         |
| 2           | Analytics         | Azure Stream Analytics   |
| 4           | Containers        | Azure Kubernetes Service |
| 5           | Containers        | Azure Service Fabric     |
| 6           | Compute           | Virtual Machines         |
| 7           | Compute           | Azure Functions          |

---

###  Expected Output

| customer\_id |
| ------------ |
| 1            |

---

###  SQL Solution

```sql
WITH TEMP AS (
    SELECT 
        A.customer_id, 
        B.product_category 
    FROM customer_contracts AS A 
    LEFT JOIN products AS B 
    ON A.product_id = B.product_id
)

SELECT customer_id 
FROM (
    SELECT customer_id, COUNT(*) AS cnt 
    FROM (
        SELECT customer_id, product_category 
        FROM TEMP 
        GROUP BY customer_id, product_category
    ) AS category_counts
    GROUP BY customer_id
) AS customer_category_count
WHERE cnt = (
    SELECT COUNT(DISTINCT product_category) 
    FROM products
);
```

---
