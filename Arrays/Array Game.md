
## Ques Link -: https://codeforces.com/problemset/problem/1904/C

> Given an array of integers, in each operation you can append the **absolute difference of any two elements**. After `k` operations, find the **minimum possible element** in the array.

---

## **Problem Summary**

* You have an array `a` of size `n`.
* You can perform `k` operations.
* In one operation, choose `i < j`, compute `|a[i] - a[j]|`, and **append** it to the array.
* After `k` operations, what is the **minimum** value in the array?

---

##  Core Idea

Appending `|ai - aj|` can **only decrease or maintain** the minimum value of the array.

The optimal strategy is to **generate the smallest possible value** using combinations of `|ai - aj|` over the operations.

But:

* Once **0** is added, it stays forever. And since all future differences involving 0 will also be 0, the final minimum is **0**.
* The **GCD** of all possible differences eventually becomes the bottleneck for how small values can go.

---

##  Case Analysis Based on `k`

###  `k > 2`

* You can perform **many operations** (even up to `10^9`), so you can construct values like:

  ```
  |a[i] - a[j]|, |a[i] - |a[j] - a[k]||, ...
  ```
* Eventually, you can generate:

  ```
  GCD of all differences in array
  ```
* And since GCD-based construction with many operations always allows you to build **0**, the final minimum will be:

  ```
   Answer = 0
  ```

### `k == 1`

* Only **one** operation is allowed.
* You can choose any pair `(i, j)` such that `i < j`, and append `|a[i] - a[j]|`.

You cannot do further combinations.

So after 1 operation:

* Your array grows by 1.
* The minimum is the **minimum of the original array and all `|a[i] - a[j]|` values**.

But instead of generating all combinations, the optimal pair will always be **closest neighbors** in sorted array.

```java
for (int i = 1; i < n; i++) {
    min = Math.min(min, arr[i] - arr[i - 1]);
}
```

This is why you do:

```java
Arrays.sort(arr);
long curr = arr[0];
for (int i = 1; i < n; i++) {
    curr = Math.min(curr, arr[i] - arr[i - 1]);
}
```

###  `k == 2`

* You can do **2 operations**.

This lets you do more combinations like:

```text
1st: |a[i] - a[j]|
2nd: |x - y| using one of previous and a[i]
```

So, with 2 operations, you're allowed to go deeper in the difference tree.

But instead of trying all combinations naively, you aim to **find the minimum constructible value** using two difference operations and check for presence of close values via binary search.

Hence this part:

```java
for each i, j: 
    curr = |a[i] - a[j]|
    check how close arr[mid] is to curr using binary search
```

---

##  Time Complexity

Let’s break it down:

### For each test case:

* Sorting: `O(n log n)`
* For `k == 1`:

  * One pass through sorted array → `O(n)`
* For `k == 2`:

  * Two nested loops `i` and `j` → `O(n^2)`
  * Binary search for each pair → `O(log n)`
  * So total: `O(n^2 log n)`

### Constraints:

* `n <= 2000`
* Total `n^2` over all test cases ≤ `4 * 10^6`

This ensures the program runs efficiently.

### So, total time per test case:

* Worst-case: `O(n^2 log n)` if `k == 2`
* Space: `O(n)` (for input array)

---

##  Space Complexity

* You use one static array `arr[2005]`
* No other large data structures used

**Space complexity**: `O(n)` per test case

---

##  Summary

| `k` Value | Strategy                               | Result         |                   |                              |    |                                |
| --------- | -------------------------------------- | -------------- | ----------------- | ---------------------------- | -- | ------------------------------ |
| `k > 2`   | Any value constructible ⇒ eventually 0 | **Answer = 0** |                   |                              |    |                                |
| `k == 1`  | Only one \`                            | a\[i]-a\[j]    | \` can be added   | Minimum of `arr[i]-arr[i-1]` |    |                                |
| `k == 2`  | Try \`                                 | a\[i] - a\[j]  | `and see closest` | x - y                        | \` | Brute-force with binary search |

---
```
import java.util.*;
 
public class Main {
    static final int maxi = 2005;
    static long[] arr = new long[maxi];
 
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();
 
        for (int ind = 0; ind < t; ind++) {
            int n = sc.nextInt();
            int k = sc.nextInt();
 
            for (int i = 0; i < n; i++) {
                arr[i] = sc.nextLong();
            }
 
            Arrays.sort(arr, 0, n); 
 
            if (k > 2) {
                System.out.println(0);
                continue;
            }
 
            if (k == 1) {
                long curr = arr[0];
                for (int i = 1; i < n; i++) {
                    curr = Math.min(curr, Math.abs(arr[i] - arr[i - 1]));
                }
                System.out.println(curr);
            } else {
                long answer = arr[0];
                for (int i = 1; i < n; i++) {
                    for (int j = 0; j < i; j++) {
                        long curr = Math.abs(arr[i] - arr[j]);
                        answer = Math.min(answer, curr);
                        int start = 0, end = n - 1;
                        while (start <= end) {
                            int mid = start + (end - start) / 2;
                            if (arr[mid] >= curr) {
                                end = mid - 1;
                            } else {
                                start = mid + 1;
                            }
                        }
                        if (start < n) answer = Math.min(answer, Math.abs(arr[start] - curr));
                        if (end >= 0) answer = Math.min(answer, Math.abs(arr[end] - curr));
                        // answer = Math.min(answer, curr);
                    }
                }
                System.out.println(answer);
            }
        }
    }
}
