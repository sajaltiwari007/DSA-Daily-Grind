## Queslink-: https://leetcode.com/problems/longest-happy-prefix/description/


### ✅ **Problem:**

Find the **longest prefix** of a string `s` that is also a **suffix** (but **not the full string itself**).

---

###  **Approach & Intuition:**

You are using the **KMP algorithm’s LPS (Longest Prefix Suffix)** array.

* `lps[i]` = the **length** of the longest **prefix** that is also a **suffix** for the substring `s[0...i]`.
* Build the LPS array:

  * If characters match, extend the prefix.
  * If not, fallback using previously computed LPS values.
* Final answer = `s.substring(0, lps[n-1])`.

This avoids comparing all substrings and runs in **linear time**.

---

###  **Key Takeaways:**

* LPS helps avoid re-computation — **KMP-style efficiency**.
* `len = lps[len - 1]` is the **smart rollback** step instead of brute force.
* This technique works **even for repeated characters**, or **overlapping patterns**.


---

###  **Complexity:**

* **Time:** `O(n)`
* **Space:** `O(n)` (for LPS array)

---

```java
// File: LongestPrefixSuffix.java

class Solution {
    public String longestPrefix(String s) {
        int n = s.length();
        int[] lps = new int[n];
        int len = 0;
        int i = 1;

        while (i < n) {
            if (s.charAt(i) == s.charAt(len)) {
                len++;
                lps[i] = len;
                i++;
            } else {
                if (len != 0) {
                    len = lps[len - 1];
                } else {
                    i++;
                }
            }
        }

        return s.substring(0, lps[n - 1]);
    }
}

