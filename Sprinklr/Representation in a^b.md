### Question Link-: https://www.geeksforgeeks.org/dsa/check-if-a-number-can-be-expressed-as-ab-set-2/

---

## 🔢 Check if a Number Can Be Expressed as a⁽ᵇ⁾ (a ≥ 2, b ≥ 2)

###  Problem

Given a number `n`, check whether it can be expressed as `a^b` where:

* `a ≥ 2`
* `b ≥ 2`
* Both `a` and `b` are integers.

---

###  Approach

* Iterate through all possible values of base `a` from `2` to `√n`.
* For each base `a`, repeatedly divide `n` by `a` while it's divisible.
* If the result becomes `1`, then `n = a^b` for some `b ≥ 2`.

---

###  Time Complexity

```
O(√n)
```

* The outer loop runs up to `√n`.
* The total number of divisions is amortized and doesn't exceed `log(n)` overall.

###  Space Complexity

```
O(1)
```

* Only a few variables used.

---

###  Sample Input/Output

```plaintext
Input:  81
Output: Yes, it can be expressed as a^b

Input:  20
Output: No, it cannot be expressed as a^b
```

---

###  Java Code

```java
import java.util.Scanner;

public class PowerCheck {
    public static boolean isPower(long n) {
        if (n <= 1) return false; // b must be >= 2

        long limit = (long)Math.sqrt(n);
        for (long a = 2; a <= limit; a++) {
            long temp = n;
            while (temp % a == 0) {
                temp /= a;
            }
            if (temp == 1) {
                return true; // n = a^b for some b >= 2
            }
        }
        return false;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        long n = sc.nextLong();

        if (isPower(n)) {
            System.out.println("Yes, it can be expressed as a^b");
        } else {
            System.out.println("No, it cannot be expressed as a^b");
        }

        sc.close();
    }
}
```


