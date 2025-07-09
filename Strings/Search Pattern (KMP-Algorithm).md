## Ques Link-: https://www.geeksforgeeks.org/problems/search-pattern0205/1
---

###  **Approach:**

You're implementing the **KMP (Knuth-Morris-Pratt)** algorithm to search for all **occurrences of a pattern** `pat` in a given text `txt`.

 **Two key steps:**

1. **Preprocess pattern** using the **LPS (Longest Prefix Suffix)** array to avoid redundant comparisons.
2. Use the **LPS array** to efficiently **slide the pattern** over the text.

---

###  **How It Works:**

* `lps[j-1]` tells you how much of the prefix you can safely skip if there's a mismatch.
* Avoids O(M\*N) brute force checking by **never moving backwards in text**.

---

###  **Time & Space Complexity:**

* **Preprocessing (LPS):** `O(M)`
* **Search:** `O(N)`
* ✅ **Total:** `O(N + M)`
* **Space:** `O(M)` for LPS array

---

###  **When to Use KMP:**

* You need to search for a **pattern multiple times** in a long string.
* You want an efficient alternative to brute force or regex matching.

---

```java
// File: KMPSearch.java

import java.util.*;

class Solution {

    public ArrayList<Integer> search(String pat, String txt) {
        ArrayList<Integer> result = new ArrayList<>();
        int N = txt.length();
        int M = pat.length();
        int[] lps = computeLPS(pat);

        int i = 0, j = 0;
        while (i < N) {
            if (j < M && pat.charAt(j) == txt.charAt(i)) {
                i++;
                j++;
            }

            if (j == M) {
                result.add(i - j); // Match found at index (1-based if needed: i - j + 1)
                j = lps[j - 1];
            } else if (i < N && (j == 0 || pat.charAt(j) != txt.charAt(i))) {
                if (j != 0) {
                    j = lps[j - 1];
                } else {
                    i++;
                }
            }
        }

        return result;
    }

    private int[] computeLPS(String pattern) {
        int M = pattern.length();
        int[] lps = new int[M];
        int len = 0;
        int i = 1;

        while (i < M) {
            if (pattern.charAt(i) == pattern.charAt(len)) {
                len++;
                lps[i] = len;
                i++;
            } else {
                if (len != 0) {
                    len = lps[len - 1];
                } else {
                    lps[i] = 0;
                    i++;
                }
            }
        }

        return lps;
    }
}
```


