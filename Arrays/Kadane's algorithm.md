## Problem Link-: https://www.geeksforgeeks.org/problems/max-circular-subarray-sum-1587115620/1

###  **Problem**:

You are given a **circular array**, and the task is to **find the maximum possible sum of a non-empty subarray**. A subarray **can wrap around** (i.e., include the last and first elements together).

---

###  **Intuition**

This problem is a **variation of Kadane’s Algorithm** which is used to find the **maximum subarray sum** in linear time.

Since the array is **circular**, there are **two cases** to consider:

#### **Case 1: The max subarray is a normal (non-circular) subarray**

This is the classic **Kadane's algorithm**, which finds the max subarray sum in O(n).

#### **Case 2: The max subarray is a circular subarray**

If the subarray **wraps around**, we can think of it as:

```
max_wraparound_sum = total_sum - min_subarray_sum
```

That’s because excluding the **minimum subarray** from the total sum gives us the maximum circular subarray.

---

###  **Code Breakdown**

```java
int answer = Integer.MIN_VALUE;
int sum = 0;
int total = 0;
```

* `answer`: Stores the max subarray sum (Kadane's)
* `total`: Sum of the entire array

---

####  **Kadane’s Algorithm for normal max subarray**:

```java
for(int i=0;i<nums.length;i++){
    total += nums[i];
    sum += nums[i];
    answer = Math.max(answer, sum);
    if(sum < 0) sum = 0;
}
```

* Classic Kadane’s Algorithm.
* If current `sum` goes below 0, reset it.
* We also keep track of total array sum.

---

####  **Kadane’s variant for min subarray** (for circular case):

```java
sum = 0;
int mini = Integer.MAX_VALUE;
for(int i=0;i<nums.length;i++){
    sum += nums[i];
    mini = Math.min(mini, sum);
    if(sum >= 0) sum = 0;
}
```

* Same as Kadane's but for **minimum subarray sum**.
* If sum becomes positive, reset (because we want to minimize).

---

####  **Final Return**:

```java
return Math.max(answer, total-mini != 0 ? total-mini : Integer.MIN_VALUE);
```

* We return the **maximum** between:

  * `answer` (non-circular case)
  * `total - mini` (circular case)
* Special Case: When `total == mini`, it means all numbers are **negative** → so `total - mini = 0`, which is invalid. In such case, we should return `answer` only.

---

###  **Time and Space Complexity**

* **Time Complexity**: `O(n)`

  * One pass for `max subarray`, one pass for `min subarray`
* **Space Complexity**: `O(1)`

  * Constant space used

---

###  **Kadane's Algorithm Use**

Kadane’s Algorithm is used **twice**:

1. **To find max subarray** (`answer`)
2. **To find min subarray** (`mini`) to compute wraparound max as `total - mini`

---

## Java Solution-:
```

class Solution {
    public int maxSubarraySumCircular(int[] nums) {

        int answer = Integer.MIN_VALUE;;
        int sum = 0;
        int total = 0;

        for(int i=0;i<nums.length;i++){
            total+=nums[i];
            sum+=nums[i];
            answer = Math.max(answer,sum);
            if(sum<0) sum=0;
        }
        // System.out.println(answer);
        sum = 0;
        int mini = Integer.MAX_VALUE;
        for(int i=0;i<nums.length;i++){
            sum+=nums[i];
            mini = Math.min(mini,sum);
            if(sum>=0) sum=0;
        }

        return Math.max(answer,total-mini!=0?total-mini:Integer.MIN_VALUE);
        

        
    }
}

