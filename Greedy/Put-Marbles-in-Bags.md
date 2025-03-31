## Problem Link -:
https://leetcode.com/problems/put-marbles-in-bags/description/?envType=daily-question&envId=2025-03-31

## Problem Statment
You have k bags. You are given a 0-indexed integer array weights where weights[i] is the weight of the ith marble. You are also given the integer k.

Divide the marbles into the k bags according to the following rules:

No bag is empty.
If the ith marble and jth marble are in a bag, then all marbles with an index between the ith and jth indices should also be in that same bag.
If a bag consists of all the marbles with an index from i to j inclusively, then the cost of the bag is weights[i] + weights[j].
The score after distributing the marbles is the sum of the costs of all the k bags.

Return the difference between the maximum and minimum scores among marble distributions.
Example 1:
Input: weights = [1,3,5,1], k = 2
Output: 4
Explanation: 
The distribution [1],[3,5,1] results in the minimal score of (1+1) + (3+1) = 6. 
The distribution [1,3],[5,1], results in the maximal score of (1+3) + (5+1) = 10. 
Thus, we return their difference 10 - 6 = 4.
Constraints:

1 <= k <= weights.length <= 105
1 <= weights[i] <= 109

## Approach 
Try to transform this question to the form what I can understand!
What is the essential meaning for a partition?
Find k-1 pairs (weights[P[i]], weights[P[i]+1]) where P[i] denotes the end index for the i-th subarray for this partition P . The score for the this partition P is
score(P)=weights[0]+weights[n−1]+∑(weights[P[i]]+weights[P[i]+1])
The wanted answer is max score(P)−min score(P).
In the above explanation the idea is to add ends of adjacent subarrays instead of the ends of the same subarray.
Then sort the sums and select the total of k - 1 from top and bottom and return the difference.

## Complexity
Time Complexity Analysis:
Building the Heap: We insert N-1 elements into the heap → O(N log N).
Extracting elements from Heap:
Extracting k-1 smallest elements → O(k log N)
Extracting k-1 largest elements (equivalent to sorting the array and picking the largest k-1) → O(N log N)
Thus, the total time complexity is O(N log N).

Space Complexity:
Heap stores N-1 elements, so O(N).
Few extra integer variables, negligible compared to the heap.
Overall, the space complexity is O(N).

## My Java Code



       class Solution {
    public long putMarbles(int[] weights, int k) {
    PriorityQueue<Long> q = new PriorityQueue<>(); // For Stroing the Sum of Pair of the ARray.

        for(int i=1;i<weights.length;i++){
            q.offer((long)((long)weights[i-1]+(long)weights[i]));
        }

        long maxi = weights[0]+weights[weights.length-1];
        long mini = weights[0]+weights[weights.length-1];

        int miniCount = 0;
        while(!q.isEmpty()){

             if(miniCount!=k-1){
                mini+=q.peek();
                miniCount++;
             }
             if(q.size()<=k-1){
                maxi+=q.peek();
             }

             q.poll();
        }

        return maxi-mini;

        
    }
}


