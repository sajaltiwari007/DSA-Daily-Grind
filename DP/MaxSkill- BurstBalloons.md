## https://www.geeksforgeeks.org/problems/burst-balloons/1 

## Approach -: Think in reverse Order
Refernce-: https://www.youtube.com/watch?v=Yz4LlDSlkns

## My Java Solution 

        
       class Solution {
    public static int maxSkill(int[] arr) {
        // code here
        int n = arr.length;
        int[] nums = new int[n+2];
        for(int i=0;i<n;i++){
            nums[i+1] = arr[i];
        }
        nums[0] = 1;
        nums[nums.length-1] = 1;
        int[][] dp = new int[nums.length+1][nums.length+1];
        for(int i=0;i<dp.length;i++) Arrays.fill(dp[i],-1);
        
        return helper(nums,0,nums.length-1,dp);
    }
    
    static int helper(int[] nums , int start , int end , int[][] dp){
        
        if(start+1==end) return 0;
        
        if(dp[start][end]!=-1) return dp[start][end];
        
        int answer = 0;
        for(int i = start+1;i<end;i++){
            
            int take = nums[start]*nums[i]*nums[end] + helper(nums,start,i,dp)+helper(nums,i,end,dp);
            answer = Math.max(answer,take);
        }
        
        return dp[start][end] = answer;
    }}
