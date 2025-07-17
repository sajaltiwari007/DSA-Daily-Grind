## Ques Link-: https://leetcode.com/problems/find-the-maximum-length-of-valid-subsequence-ii/?envType=daily-question&envId=2025-07-17

---

###  Problem Description

Given an integer array `nums` and a positive integer `k`, find the **length of the longest valid subsequence** where every pair of consecutive elements `(a, b)` in the subsequence satisfies:

```
(a + b) % k == constant
```

---

###  Intuition

We use **Dynamic Programming** to track the longest valid subsequences ending at each index with a specific modulo value. For each pair `(j, i)`, we calculate `(nums[i] + nums[j]) % k` and update the DP table accordingly. This helps in chaining elements while maintaining the required modulo condition.

---

###  Time & Space Complexity

* **Time Complexity:** `O(n^2)` — for all pairs `(j, i)` where `j < i`.
* **Space Complexity:** `O(n * k)` — DP table to store lengths for each index and modulo.

---

###  Code

```java
class Solution {
    public int maximumLength(int[] nums, int k) {
        int n = nums.length;
        int[][] dp = new int[n][k];
        int answer = 0;

        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                int mod = (nums[i] + nums[j]) % k;
                dp[i][mod] = Math.max(dp[i][mod], dp[j][mod] + 1);
                answer = Math.max(answer, dp[i][mod]);
            }
        }
        return answer + 1;
    }
}
```


