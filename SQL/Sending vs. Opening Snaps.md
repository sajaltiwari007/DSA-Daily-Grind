## Problem Link -: https://datalemur.com/questions/time-spent-snaps
### Problem Description

You're given tables with information about Snapchat user activity and their age groups. The goal is to calculate, **for each age group**, the **percentage of time spent sending snaps** and **opening snaps**, relative to the total time spent on these two activities.

* Ignore the `'chat'` activity.
* The results should be grouped by `age_bucket`.
* Round each percentage to **2 decimal places**.
* Use `100.0` instead of `100` to avoid integer division.

---

### Approach

1. First, **aggregate the time spent per user** on `'send'` and `'open'` activities.
2. Calculate the percentage time spent on each activity relative to the total (`send + open`) **per user**.
3. Join this user-level data with the `age_breakdown` table.
4. Group by `age_bucket` if needed (though this query preserves per-user granularity joined with age).

> Note: This is based on Problem #25 from the **Ace the Data Science Interview** SQL chapter.

---

### Tables Used

#### `activities`

| Column         | Type                                  |
| -------------- | ------------------------------------- |
| activity\_id   | integer                               |
| user\_id       | integer                               |
| activity\_type | string (`'send'`, `'open'`, `'chat'`) |
| time\_spent    | float                                 |
| activity\_date | datetime                              |

#### `age_breakdown`

| Column      | Type                                |
| ----------- | ----------------------------------- |
| user\_id    | integer                             |
| age\_bucket | string (`'21-25'`, `'26-30'`, etc.) |

---
###  Output Example

| age\_bucket | send\_perc | open\_perc |
| ----------- | ---------- | ---------- |
| 26-30       | 65.40      | 34.60      |
| 31-35       | 43.75      | 56.25      |

---

### ✅ SQL Solution

```sql
SELECT H2.age_bucket , H1.send_perc , H1.open_perc
FROM 
(
  SELECT 
    A.user_id,
    ROUND(
      SUM(CASE WHEN A.activity_type = 'open' THEN A.time_spent ELSE 0 END) * 100.0 /
      NULLIF(SUM(CASE WHEN A.activity_type IN ('send', 'open') THEN A.time_spent ELSE 0 END), 0),
      2
    ) AS open_perc,  

    ROUND(
      SUM(CASE WHEN A.activity_type = 'send' THEN A.time_spent ELSE 0 END) * 100.0 /
      NULLIF(SUM(CASE WHEN A.activity_type IN ('send', 'open') THEN A.time_spent ELSE 0 END), 0),
      2
    ) AS send_perc
  FROM activities AS A
  GROUP BY A.user_id
) AS H1 
LEFT JOIN age_breakdown AS H2 
  ON H1.user_id = H2.user_id
ORDER BY H2.age_bucket;
```

---


