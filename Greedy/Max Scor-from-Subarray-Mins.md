
## Question Link-: https://www.geeksforgeeks.org/problems/max-sum-in-sub-arrays0824/1
## Question Description-: 
You are given an array arr[] of integers. Your task is to find the maximum sum of the smallest and second smallest elements across all subarrays (of size >= 2) of the given array.
## Strategy-:: Observation. Maximum sum for smallest and 2nd smallest always comes from subarray of size 2 i.e the sum of adjacent elements.

## Java Solution



        
        
       class Solution {
    public int maxSum(int arr[]) {
        // code here
        int maxi = -1;
       for(int i=1;i<arr.length;i++) maxi = Math.max(maxi,arr[i]+arr[i-1]);
       return maxi;
     }}
