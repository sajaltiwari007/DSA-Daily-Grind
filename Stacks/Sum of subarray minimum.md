## Question Link-: https://www.geeksforgeeks.org/problems/sum-of-subarray-minimum/1

### **Key Concepts & Intuition:**

This problem focuses on finding the **sum of the minimums of all subarrays** in an array.
Key idea is to determine **how many subarrays** an element is **minimum in** and then **multiply it by its value**.

---

###  **Core Intuition:**

* For each element `arr[i]`, calculate:

  * **`left[i]`** = number of elements to the **left** (including itself) where it is the **minimum**.
  * **`right[i]`** = number of elements to the **right** (including itself) where it is the **minimum**.
* Then its contribution = `arr[i] * leftCount * rightCount`

This is done efficiently using **monotonic stacks**.

---

### **What You Learn / Takeaways:**

* Usage of **monotonic stack** to find **Previous Less Element (PLE)** and **Next Less Element (NLE)**.
* How to **count subarrays** where a particular element is the **minimum**.
* How to **optimize brute force subarray problems** using structure-based logic.

---

###  **Time & Space Complexity:**

* **Time:** `O(n)` for both left and right pass.
* **Space:** `O(n)` for the stacks and arrays.

---

### **Main Formula:**

For each element `arr[i]`, its **total contribution** to the sum of minimums of all subarrays is:

$$
\text{contribution} = \text{arr[i]} \times \text{leftCount} \times \text{rightCount}
$$

Where:

* `leftCount = i - left[i] + 1`
* `rightCount = right[i] - i + 1`

So the formula becomes:

$$
\text{answer} += \text{arr[i]} \times (i - \text{left}[i] + 1) \times (\text{right}[i] - i + 1)
$$

---

###  **Why This Works:**

We're finding **how many subarrays include `arr[i]` as the minimum**.

* `left[i]` is the **nearest previous index** where an element **smaller than `arr[i]`** exists.
* So all indices from `left[i]` to `i` can form a **left boundary**.
* Similarly, `right[i]` is the nearest next index with a **smaller element**, so all indices from `i` to `right[i]` can form a **right boundary**.

Total number of subarrays where `arr[i]` is the **minimum** =
`(i - left[i] + 1)` choices on the **left**,
`(right[i] - i + 1)` choices on the **right**.

So total subarrays =

$$
\text{leftCount} \times \text{rightCount}
$$

Each of these subarrays has `arr[i]` as the **minimum**, so total contribution of `arr[i]` =

$$
\text{arr[i]} \times \text{leftCount} \times \text{rightCount}
$$

---



```java
// File: SumOfSubarrayMinimums.java

import java.util.*;

class Solution {
    public int sumSubMins(int[] arr) {
        int n = arr.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Stack<Integer> stack = new Stack<>();

        // Find Next Less Element (Right boundary)
        for (int i = n - 1; i >= 0; i--) {
            while (!stack.isEmpty() && arr[stack.peek()] > arr[i]) {
                stack.pop();
            }
            right[i] = stack.isEmpty() ? n - 1 : stack.peek() - 1;
            stack.push(i);
        }

        stack.clear();

        // Find Previous Less Element (Left boundary)
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && arr[stack.peek()] >= arr[i]) {
                stack.pop();
            }
            left[i] = stack.isEmpty() ? 0 : stack.peek() + 1;
            stack.push(i);
        }

        int answer = 0;
        int mod = (int) 1e9 + 7; // Optional, if modulo required

        for (int i = 0; i < n; i++) {
            long count = (long) (right[i] - i + 1) * (i - left[i] + 1);
            answer = (int) ((answer + count * arr[i]) % mod);
        }

        return answer;
    }
}
```

