## https://www.geeksforgeeks.org/problems/smallest-positive-missing-number-1587115621/1



### **Intuition**

In this labyrinth of numbers, we know that the smallest missing positive integer lies **between 1 and n + 1** (where *n* is the array length). So, the algorithm attempts to place each valid number `x` (where `1 ≤ x ≤ n`) at index `x - 1`.

This transforms the array into a **tapestry** of well-ordered victuals, making it easy to spot where the pattern breaks.

---

###  **Approach**

1. **Cyclic Sort Style**:
   While traversing, if `nums[i]` is in range `[1, n]` and not already in its right spot (`nums[nums[i] - 1] ≠ nums[i]`), we swap. This dance of swaps continues until each valid number is nestled into its rightful index.

2. **Scan for First Gap**:
   Then, we embark again linearly. The first index `i` where `nums[i] ≠ i + 1` reveals the enigmatic missing number—`i + 1`.

3. **If All in Place**:
   The array is a perfect kaleidoscopic match—so return `n + 1`.

---

###  **Time Complexity**

* **O(n)**: Each element is moved at most once in the first loop and traversed once more in the second—no redundant steps in this crucible.

### **Space Complexity**

* **O(1)**: Everything is done in-place, memory remains untouched.

---

## Solution-1: O(N) time O(N) SPACE
```
class Solution {
    public int firstMissingPositive(int[] nums) {

        boolean[] exits = new boolean[100001];
        for(int i:nums){
            if(i>0 && i<exits.length) exits[i] = true;
        }

        for(int i=1;i<exits.length;i++){
            if(!exits[i]){
                return i;
            }
        }
        return 100001;
        
    }
}
````
### Solution 2-: O(N) time O(1) Space

```
class Solution {
    public int firstMissingPositive(int[] nums) {

        int i=0;
        
        while(i<nums.length){
            
            if(nums[i]<=0 || i+1==nums[i]) i++;
            else{
                int ind = nums[i]-1;
                if(ind>=nums.length || nums[ind]==nums[i]){
                    i++;
                    continue;
                }
                int temp = nums[ind];
                nums[ind] = nums[i];
                nums[i] = temp;
            }
        }
        i=0;
        while(i<nums.length){
            if(i+1==nums[i]){
                i++;
            }
            else{
                return i+1;
            }
        }
        
        return nums.length+1;
        
    }
}
