##  Uber Transactions – Find Third Transaction of Every User

###  Problem Statement

Given a table `transactions` containing Uber transaction logs, **write a SQL query to return the 3rd transaction (chronologically) for every user**.

### Output

Return the following columns for each qualifying user:

* `user_id`
* `spend`
* `transaction_date`

---

### Example Table: `transactions`

| user\_id | spend | transaction\_date |
| -------- | ----- | ----------------- |
| 1        | 100   | 2024-01-01        |
| 1        | 150   | 2024-01-03        |
| 1        | 200   | 2024-01-05        |
| 2        | 50    | 2024-02-01        |
| 2        | 70    | 2024-02-10        |
| 2        | 90    | 2024-03-01        |
| 3        | 80    | 2024-01-01        |

---

### Expected Output

| user\_id | spend | transaction\_date |
| -------- | ----- | ----------------- |
| 1        | 200   | 2024-01-05        |
| 2        | 90    | 2024-03-01        |

---

###  Explanation

* We want the **3rd transaction by date** for each user.
* User `1` has 3 transactions → output the 3rd: `200 on 2024-01-05`.
* User `2` has 3 transactions → output the 3rd: `90 on 2024-03-01`.
* User `3` has only 1 transaction → not included.

---

### Solution (using self-joins)

```sql
SELECT A.USER_ID, A.SPEND, A.TRANSACTION_DATE 
FROM 
(
    SELECT ROW_NUMBER() OVER(ORDER BY USER_ID) AS XYZ, *
    FROM (
        SELECT * FROM transactions ORDER BY user_id, transaction_date
    ) AS TAB
) AS A, 
(
    SELECT ROW_NUMBER() OVER(ORDER BY USER_ID) AS XYZ, *
    FROM (
        SELECT * FROM transactions ORDER BY user_id, transaction_date
    ) AS TAB
) AS B, 
(
    SELECT ROW_NUMBER() OVER(ORDER BY USER_ID) AS XYZ, *
    FROM (
        SELECT * FROM transactions ORDER BY user_id, transaction_date
    ) AS TAB
) AS C 
WHERE A.XYZ-1 = B.XYZ 
  AND B.XYZ-1 = C.XYZ 
  AND A.USER_ID = B.USER_ID 
  AND B.USER_ID = C.USER_ID;
```

### Solution Using `ROW_NUMBER()`

```sql
SELECT user_id, spend, transaction_date
FROM (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY transaction_date) AS rn
    FROM transactions
) t
WHERE rn = 3;
```



---

Let me know if you want the same format for LeetCode, HackerRank, etc., or want to include file headers/comments for GitHub!



