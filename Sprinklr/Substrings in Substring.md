### 📘 Problem Description

You are given a 1-indexed string `S` of length `N` consisting only of lowercase Latin letters. A substring is called **good** if it starts and ends with the same character. Single characters are also considered good substrings.

You are required to answer `Q` queries. Each query is of the form:

```
l r
```

Which asks for the number of good substrings in the substring `S[l...r]`.

---
### ✅ Input Format

* The first line contains an integer `T`, the number of test cases.
* For each test case:

  * The first line contains two integers `N` and `Q`.
  * The second line contains a string `S` of length `N`.
  * Each of the next `Q` lines contains two integers `l` and `r`.

---

###  Output Format

For each query, print a single line containing the number of good substrings in `S[l...r]`.
---

### 📌 Sample Input

```
2
4 2
aabc
1 2
3 4
6 4
abcaab
1 1
2 5
3 6
1 6
```

###  Sample Output

```
3
2
1
5
5
10
```

---


###  Approach

We use prefix sums to preprocess the character frequencies at each index. This allows us to answer each query in **O(26)** time:

1. Create a 2D array `freq[i][c]` where `c` is the character index (`0` to `25`).
2. For each character in the string, update the frequency at the corresponding index.
3. Compute prefix sums so that the frequency of character `c` in `S[l...r]` is `freq[r][c] - freq[l - 1][c]`.
4. For each query, compute the total number of good substrings by summing `f * (f + 1) / 2` for each character frequency `f`.

```java
import java.util.*;

public class GoodSubstrings {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int t = sc.nextInt();  // Number of test cases
        while (t-- > 0) {
            int n = sc.nextInt();  // Length of the string
            int q = sc.nextInt();  // Number of queries
            String s = sc.next();  // The input string

            // Prefix frequency array where freq[i][j] denotes the frequency
            // of character 'a' + j in the prefix [1...i] of the string
            long[][] freq = new long[n + 2][26];
            for (int i = 1; i <= n; i++) {
                freq[i][s.charAt(i - 1) - 'a']++;
            }

            // Build prefix sums for character frequencies
            for (int i = 1; i <= n; i++) {
                for (int j = 0; j < 26; j++) {
                    freq[i][j] += freq[i - 1][j];
                }
            }

            // Process each query
            for (int i = 0; i < q; i++) {
                int l = sc.nextInt();
                int r = sc.nextInt();

                long answer = 0;
                // For each character from 'a' to 'z', count how many times it appears in S[l...r]
                for (int j = 0; j < 26; j++) {
                    long count = freq[r][j] - freq[l - 1][j];
                    // Number of good substrings for a character with count c is c * (c + 1) / 2
                    answer += (count * (count + 1)) / 2;
                }

                System.out.println(answer);
            }
        }

        sc.close();
    }
}
```

---

### ⏱ Time and Space Complexity

* **Time Complexity**:

  * Preprocessing prefix sums: `O(N * 26)`
  * Each query: `O(26)`
  * Total across all test cases: `O((N + Q) * 26)` (within limits)

* **Space Complexity**: `O(N * 26)` for the prefix sum array



