## Ques Link-: https://www.geeksforgeeks.org/problems/nine-divisors3751/1
---

##  Problem Description

**Given** a positive integer `n`, **count how many numbers** less than or equal to `n` have **exactly 9 divisors**.

---

##  Key Intuition

To solve this, we need to understand how the **number of divisors of a number** is determined using its **prime factorization**.

For a number `N = p₁^a × p₂^b × ... × pₖ^z`,
→ The total number of divisors is `(a+1) × (b+1) × ... × (z+1)`.

We are interested in numbers where the **number of divisors equals 9**.
So, we look for exponent combinations such that:

```
(a+1)(b+1)... = 9
```

###  Only valid factorizations of 9 are:

1. `9` → implies `(8+1)` → `N = p^8`
2. `3×3` → implies `(1+1)(1+1)` → `N = p^2 × q^2` where `p ≠ q` and both are primes.

Hence, we only need to find:

* **All numbers of the form `p^8` ≤ n** (where `p` is prime)
* **All numbers of the form `p^2 * q^2` ≤ n** (where `p ≠ q` are distinct primes)

This reduces the problem to a **prime generation task** and checking powers of primes efficiently.

---

##  Approach

1. Use the **Sieve of Eratosthenes** to generate all primes up to `√n`.
2. For every prime `p`, check if `p^8 ≤ n`.
3. For every pair of distinct primes `(p, q)`, check if `p^2 * q^2 ≤ n`.

These are the **only forms** that result in exactly 9 divisors.

---

##  Time Complexity

* **Sieve:** `O(√n log log n)`
* **Pair check for `p² * q²`:** `O(k²)` where `k` is number of primes ≤ √n
* Efficient for large `n` (up to 10⁹)

---

## Code

```java
class Solution {
    public static int countNumbers(int n) {
        int size = (int)Math.sqrt(n);
        boolean[] prime = new boolean[size+1];
        prime[0] = true;
        prime[1] = true;
        
        for(int i=2;i<size;i++){
            if(!prime[i]){
                for(int j=i*2;j<size;j=j+i){
                    prime[j] = true;
                }
            }
        }
        
        List<Integer> primes = new ArrayList<>();
        for(int i=0;i<size;i++){
            if(!prime[i]) primes.add(i);
        }
        
        int answer = 0;
        
        for(int i=0;i<primes.size();i++){
            long a = (long)primes.get(i);
            for(int j=i+1;j<primes.size();j++){
                long b = (long)primes.get(j);
                long aa = (long)a*a;
                b = (long)b*b;
                if(aa > n || b > n) continue;
                long pro = (long)aa * b;
                if(pro <= n){
                    answer++;
                }
            }
        }
        
        for(int i=0;i<primes.size();i++){
            long pro = (long)Math.pow((long)primes.get(i), 8);
            if(pro >= 0 && pro <= n){
                answer++;
            }
        }
        
        return answer;
    }
}
```

