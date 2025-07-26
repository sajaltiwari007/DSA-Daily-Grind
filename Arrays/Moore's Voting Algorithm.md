## Problem Link-: https://www.geeksforgeeks.org/problems/majority-element-1587115620/1


---

###  Intuition of Moore’s Voting Algorithm:

Imagine you're in a room full of people shouting numbers (elements in the array). You're trying to figure out if **one of them appears more than half the time**.

Now think of this process like a **vote-based fight**:

* Every time someone says the **same number** as your current favorite (candidate), it's like they are **supporting** your candidate → you increase support.
* If someone says a **different number**, it's like an **opposing vote** → you cancel out a support.

Here’s the key insight:

> If an element is the **true majority (appears more than half the time)**, then **even after pairing and canceling it out** with every other different element, it will still be **left standing at the end**.

Why?

* Because the majority element has more than n/2 votes, it can **withstand being "voted out"** by all the minority elements combined.

This cancellation strategy helps us **eliminate non-majority elements** efficiently, so only the potential majority element is left.

---

###  Why We Need Two Passes:

* First pass: Get a **potential candidate** by canceling others.
* Second pass: Actually **count** and verify if it is indeed the majority.

---

### Time Complexity:

* O(n): One pass to find the candidate + one pass to count.

###  Space Complexity:

* O(1): We only use a few variables — no extra memory or arrays.

---
```
class Solution {
    int majorityElement(int arr[]) {
        // code here
        
        int curr = arr[0];
        int cnt = 1;
        for(int i=1;i<arr.length;i++){
            if(cnt==0){
                curr = arr[i];
                cnt = 1;
                // continue;
            }
            else if(arr[i]==curr){
                cnt++;
            }
            else{
                cnt--;
                if(cnt==0) curr = -1;
            }
        }
        cnt = 0;
        for(int i=0;i<arr.length;i++){
            if(arr[i]==curr) cnt++;
        }
        return cnt>arr.length/2?curr:-1;
        
        
    }
}
```
## Problem Link-; https://www.geeksforgeeks.org/problems/majority-vote/1


---

### ✅ Problem Goal:

Find all elements in the array that occur **more than n/3 times**.

---

###  Key Difference from the original version:

In the original Moore’s Voting Algorithm (for **n/2**), you only track **one candidate** because:

* There can only be **one element** that appears more than n/2 times.

But in the **n/3 version**:

* **At most 2 elements** can appear more than n/3 times.
* So, you now need to track **two candidates** (`curr1`, `curr2`) and their vote counts.

---

###  Why the extra conditions?

1. **Two candidates needed**:
   To account for the possibility of two majority elements (`> n/3` times), you must track two separate values with independent counts.

2. **Check for collision**:

   * You avoid setting `curr1` and `curr2` to the same value.
   * You also avoid reusing `cnt1` or `cnt2` for elements that have already been assigned to the other candidate.

3. **Final verification**:

   * Like before, the first pass **gives possible candidates**.
   * Second pass **verifies** their actual frequency.

---

###  Why only **2** possible answers?

Because if an element appears more than **n/3 times**, there can be **at most 2 such elements**:

* Suppose 3 elements each appear more than n/3 → total > n → impossible.

So the algorithm:

* Tracks **two candidates**
* Uses the same **vote cancellation strategy** (like before)
* Then verifies them with a count

---

###  Time & Space Complexity:

* **Time**: O(n) — one pass for voting, one for counting
* **Space**: O(1) — only variables used (result list doesn’t count as extra space per constraints)

---
```
class Solution {
    public ArrayList<Integer> findMajority(int[] arr) {
        // Code here
        int curr1 = Integer.MIN_VALUE;
        int cnt1 = 0;
        int curr2 = Integer.MIN_VALUE;
        int cnt2 = 0;
        for(int i=0;i<arr.length;i++){
            if(cnt1==0 && curr2!=arr[i]){
                curr1 = arr[i];
                cnt1 = 1;
                // continue;
            }
            else if(cnt2==0 && curr1!=arr[i]){
                cnt2=1;
                curr2 = arr[i];
            }
            else if(arr[i]==curr1){
                cnt1++;
            }
            else if(arr[i]==curr2){
                cnt2++;
            }
            else{
                cnt1--;
                cnt2--;
                
            }
        }
        ArrayList<Integer> list = new ArrayList<>();
        cnt1=0;
        cnt2=0;
        for(int i=0;i<arr.length;i++){
            if(arr[i]==curr1){
                cnt1++;
            }
            else if(arr[i]==curr2){
                cnt2++;
            }
        }
        if(cnt1>arr.length/3) list.add(curr1);
        if(cnt2>arr.length/3) list.add(curr2);
        Collections.sort(list);
        
        return list;
    }
}




