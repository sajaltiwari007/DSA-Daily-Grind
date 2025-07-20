## Question Link-: https://leetcode.com/problems/longest-duplicate-substring/


---

### **Problem Goal:**

Given a string `s`, find the **longest substring** that appears **at least twice** in `s`.

---

##  **Approach Overview:**

Your solution uses a combination of:

*  **Binary Search**: To search the length of the longest duplicate substring.
* **Rolling Hash (Rabin-Karp)**: To check if a substring of a given length exists more than once.
*  **Double Hashing**: To reduce hash collisions and false positives.

---

##  **Intuition:**

###  Why Binary Search?

You're looking for the **maximum length L** such that a duplicate substring of length L exists. Instead of checking every length from 1 to n, we **binary search** on the length.

* At each step, you check: *"Is there any duplicate of length = mid?"*
* This check is done using rolling hash.

---

###  Why Rolling Hash?

Instead of comparing every pair of substrings (which is slow), we **hash substrings** and compare their hash values.

* Each character is treated as a number: `'a' → 0, 'b' → 1, ..., 'z' → 25`.

* A substring becomes a number in **base 26** or **base 27**:

  $$
  "abc" \to (0 \cdot 26^2 + 1 \cdot 26^1 + 2 \cdot 26^0)
  $$

* With **rolling hash**, we can compute the hash of the next window in O(1), by:

  * Removing the leftmost character's contribution
  * Shifting left (multiplying by base)
  * Adding the new character at the end

---

###  Why **Double Hashing**?

Hash **collisions** can happen: two different strings may have the **same hash value**, especially with large inputs.

So you use **two independent hash functions**:

* One with base = 26, mod = 1e9+7
* Another with base = 27, mod = 1e9+33

A match is considered valid **only if both hashes match**.

this **drastically reduces the chance of collision**, improving correctness without string comparison.

---

## ⚙ **How the Code Works:**

1. **Precompute powers** of base-26 and base-27 modulo their respective mods.

2. Use **binary search** from `1` to `n-1` (cannot have a duplicate of full string).

3. For each length `mid`, call `helper()`:

   * Use rolling hash to slide over substrings of length `mid`.
   * Store seen hashes in two sets (`set1` and `set2`).
   * If **both hash values match** for any future substring, it's a duplicate.

4. Keep track of the **longest match found** (`maxi`, `ss`, `e`).

---

##  **Time and Space Complexity:**

###  Time Complexity:

* Binary search over lengths: `O(log N)`
* Rolling hash (each helper call): `O(N)`
* HashSet operations: `O(1)` on average per insert/check

**Total:**

$$
O(N \cdot \log N)
$$

### 🧠 Space Complexity:

* `O(N)` for storing:

  * Powers of base (2 arrays)
  * Hash sets for hash values

**Total:**

$$
O(N)
$$

---

## Java Solution
```
// Double HashIng!!!!!
// First hashing-> 26 and 10^9+7
// Second hashing-> 27 and 10^9 + 33;

class Solution {
    int maxi = 0;
    int ss = -1;
    int e = -1;
    static int mod1 = 1000000007;
    static int mod2 = 1000000033;
    long[] arr1;
    long[] arr2;
    public void fillArray1(long[] arr , int n){
        arr[0] = 1L;
        for(int i=1;i<n;i++){
            arr[i] = (arr[i-1]*(long)26)%mod1;
        }
    }
    public void fillArray2(long[] arr , int n){
        arr[0] = 1L;
        for(int i=1;i<n;i++){
            arr[i] = (arr[i-1]*(long)27)%mod2;
        }
    }
    public String longestDupSubstring(String s) {
        
        int n = s.length();
        arr1 = new long[n];
        arr2 = new long[n];
        fillArray1(arr1,n);
        fillArray2(arr2,n);
        int start = 1;
        int end = n-1;
        while(start<=end){
            int mid = start + (end-start)/2;
            if(helper(s,mid)){
                maxi = mid;
                start = mid+1;
            }
            else{
                end = mid-1;
            }
        }
        if(maxi==0) return "";
        return s.substring(ss,e+1);
  
    }
    boolean helper(String s , int len){
        if(len==0) return true;
        String str = s.substring(0,len);
        long hash1 = getHash1(str,len);
        long hash2 = getHash2(str,len);
        HashSet<Long> set1 = new HashSet<>();
        HashSet<Long> set2 = new HashSet<>();
        set1.add(hash1);
        set2.add(hash2);
        int start = 0;
        int end = len;
        while(end<s.length()){
            char c = s.charAt(end);
            if(end-start+1>len){

                hash1 = (hash1-((long)(s.charAt(start)-'a')*arr1[len-1]%mod1)+mod1)%mod1;
                hash1 = ((hash1*(long)26)%mod1);
                hash2 = (hash2-((long)(s.charAt(start)-'a')*arr2[len-1]%mod2)+mod2)%mod2;
                hash2 = ((hash2*(long)27)%mod2);
                start++;
            }
            hash1 = (hash1+(c-'a')+mod1)%mod1;
            hash2 = (hash2+(c-'a')+mod2)%mod2;
            if(set1.contains(hash1) && set2.contains(hash2)){
                ss = start;
                e = end;
                return true;
            }
            set1.add(hash1); set2.add(hash2);
            end++;
        }
        return false;
    }
    public long getHash1(String s,int len){

        long answer = 0;
        for(int i=0;i<len;i++){
            answer = (answer + ((long)(s.charAt(i)-'a')*arr1[len-1-i]%mod1+mod1)%mod1)%mod1;
        }
        return answer;
    }
    public long getHash2(String s,int len){

        long answer = 0;
        for(int i=0;i<len;i++){
            answer = (answer + ((long)(s.charAt(i)-'a')*arr2[len-1-i]%mod2+mod2)%mod2)%mod2;
        }
        return answer;
    }
}
