GFG PROBLEM LINK-: https://www.geeksforgeeks.org/problems/matrix-chain-multiplication0303/1
Refer-: https://www.youtube.com/watch?v=vRVfmbCFW7Y

## Problem Description-: Given an array arr[] which represents the dimensions of a sequence of matrices where the ith matrix has the dimensions (arr[i-1] x arr[i]) for i>=1, find the most efficient way to multiply these matrices together. The efficient way is the one that involves the least number of multiplications.

Examples:

Input: arr[] = [2, 1, 3, 4]
Output: 20
Explanation: There are 3 matrices of dimensions 2 × 1, 1 × 3, and 3 × 4, Let this 3 input matrices be M1, M2, and M3. There are two ways to multiply: ((M1 x M2) x M3) and (M1 x (M2 x M3)), note that the result of (M1 x M2) is a 2 x 3 matrix and result of (M2 x M3) is a 1 x 4 matrix. 
((M1 x M2) x M3)  requires (2 x 1 x 3) + (2 x 3 x 4) = 30 
(M1 x (M2 x M3))  requires (1 x 3 x 4) + (2 x 1 x 4) = 20. 
The minimum of these two is 20.
Input: arr[] = [1, 2, 3, 4, 3]
Output: 30
Explanation: There are 4 matrices of dimensions 1 × 2, 2 × 3, 3 × 4, 4 × 3. Let this 4 input matrices be M1, M2, M3 and M4. The minimum number of multiplications are obtained by ((M1 x M2) x M3) x M4). The minimum number is (1 x 2 x 3) + (1 x 3 x 4) + (1 x 4 x 3) = 30.
Input: arr[] = [3, 4]
Output: 0
Explanation: As there is only one matrix so, there is no cost of multiplication.
Constraints: 
2 ≤ arr.size() ≤ 100
1 ≤ arr[i] ≤ 200

Approach
Implemented using a recursive approach with memoization to optimize redundant calculations.
A 2D dp array stores intermediate results to avoid recomputation.
The function helper() recursively finds the optimal partition to minimize the cost of matrix multiplication.

Complexity
Time Complexity:O(n^3)
Space Complexity: O(n^2)

## MY JAVA SOLUTION USING DP 


   
    class Solution{
    static int matrixMultiplication(int arr[]) {
        
         code here
        
        int[][] dp = new int[arr.length+1][arr.length+1];
        for(int i=0;i<dp.length;i++) Arrays.fill(dp[i],-1);
        return helper(arr,1,arr.length-1,dp);
    }
    static int helper(int[] arr , int start , int end , int[][] dp){
        if(start>=end) return 0;
        if(dp[start][end]!=-1) return dp[start][end];
        int mini = Integer.MAX_VALUE;
        for(int k=start;k<end;k++){
            int answer = arr[start-1]*arr[k]*arr[end] + helper(arr,start,k,dp) + helper(arr,k+1,end,dp);
            mini = Math.min(mini,answer);
        }
        
        return dp[start][end] = mini;
    }}
