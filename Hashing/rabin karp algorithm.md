## Question Link-: https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/


---

###  **Approach:**

We use **Rabin-Karp (rolling hash)** to find the first occurrence of `needle` in `haystack`.

#### Steps:

1. **Hashing strings:**

   * Each character `'a'` to `'z'` is treated as a number from `0` to `25`.
   * A string becomes a number in base-26. For example:

     ```
     "abc" → (0 * 26² + 1 * 26¹ + 2 * 26⁰)
     ```
2. **Precompute powers of 26 mod `1e9+7`** to speed up hash calculation.
3. **Get hash of `needle`**.
4. Calculate the hash of the **first window** (length = needle) in `haystack`.
5. Then, slide the window one character at a time:

   * **Update the hash in O(1)** using:

     * Remove leftmost char's effect.
     * Shift hash (multiply by 26).
     * Add new char at the right.
6. If the rolling hash matches `needle`'s hash → potential match.

   * (In full Rabin-Karp, we also compare strings to confirm. You skipped this step for speed.)

---

###  **Why hashing works:**

Hashing converts a string into a unique number (ideally). Comparing two strings becomes comparing two numbers — much faster.

To avoid recomputing hash from scratch for every window, we update it **incrementally** using the precomputed powers — that's the "rolling" part.

---

###  **Complexity:**

* **Time:** `O(m)` — process haystack in one pass with O(1) updates.
* **Space:** `O(n)` — store powers of 26 up to `needle.length()`.

---

## Brute Solution 
```
class Solution {
    public int strStr(String haystack, String needle) {
        int l = needle.length(); int k = 0,t=0;
        char c = needle.charAt(0);
        String s = "";
        for(int i=0;i<=haystack.length()-l;i++)
        {
            if(c==haystack.charAt(i))
            { 
                s = haystack.substring(i,i+l);
                
                if(s.equals(needle))
                {
                    k++;
                    t=i;
                    break;
                    
                }
                
            }
        }
        if(k==0)
            return -1;
        else
            return t;
    }
}

```

## Optimal Solution using Hashing
```
// O(N) Solution

class Solution {
    static int mod = 1000000007;
     long[] arr;

    void fillArray(long[] arr , int n){
        arr[0] = 1L;
        for(int i=1;i<n;i++){
            arr[i] = ((long)arr[i-1]*(long)26)%mod;
        }

    }
    public int strStr(String haystack, String needle) {

        //Hash Funcion
        int n = needle.length();
        if(haystack.length()<n) return -1;
        arr = new long[n];
        fillArray(arr,n);
        long needle_hash = getHash(needle,n);
        String s = haystack.substring(0,n);
        long hash = getHash(s,n);
        if(needle_hash==hash) return 0;
        int start=0;
        int end = n;
        while(end<haystack.length()){

            char c = haystack.charAt(end);
            if(end-start+1>n){
                hash = (hash-((haystack.charAt(start)-'a')*arr[n-1]%mod+mod)%mod+mod)%mod;
                hash = (hash*(long)26)%mod;
                start++;
            }
            hash = (hash+(c-'a'))%mod;
            if(needle_hash==hash) return start;
            end++;
        }
        return -1;
        
    }
    public long getHash(String s,int n){
         
         long hash = 0L;
         for(int i=0;i<n;i++){
            hash = (hash+((long)(s.charAt(i)-'a')*arr[n-1-i])%mod+mod)%mod;
         }
         return hash;
        
    }
}
