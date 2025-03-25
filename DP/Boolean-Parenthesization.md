## Problem Link-:  https://www.geeksforgeeks.org/problems/boolean-parenthesization5610/1?_gl=1*1nmk4g2*_up*MQ..*_gs*MQ..&gclid=Cj0KCQjwqIm_BhDnARIsAKBYcmstUXpSDUpccd5b4d2txx7pyNwSvXT5STCUDFmFdGol2sWcWm0uNT8aAhVeEALw_wcB
## Refernce -: https://www.youtube.com/watch?v=MM7fXopgyjw
## Problem Description
You are given a boolean expression s containing
    'T' ---> true
    'F' ---> false 
and following operators between symbols
   &   ---> boolean AND
    |   ---> boolean OR
   ^   ---> boolean XOR
Count the number of ways we can parenthesize the expression so that the value of expression evaluates to true.

Note: The answer is guaranteed to fit within a 32-bit integer.

Examples:

Input: s = "T|T&F^T"
Output: 4
Explaination: The expression evaluates to true in 4 ways: ((T|T)&(F^T)), (T|(T&(F^T))), (((T|T)&F)^T) and (T|((T&F)^T)).
Input: s = "T^F|F"
Output: 2
Explaination: The expression evaluates to true in 2 ways: ((T^F)|F) and (T^(F|F)).
Constraints:
1 ≤ |s| ≤ 100 
## Approach
Approach and Intuition
Recursive Breakdown:
The string is divided into smaller subproblems using the operators (&, |, ^) as partition points.
For each operator, the number of ways to evaluate the left and right sides to both True and False is calculated using recursion.

State Representation:
The function helper(s, start, end, isTrue, dp) is defined where:
start → Start index of the substring.
end → End index of the substring.
isTrue → Boolean representing if the expression needs to be evaluated as True (1) or False (0).
dp → A 3D array for memoization to store already computed results.

Logical Operations:
Depending on the operator (&, |, ^), the number of valid parenthesizations is calculated using:
lt → Left side evaluates to True
lf → Left side evaluates to False
rt → Right side evaluates to True
rf → Right side evaluates to False

The combinations are calculated based on logical rules:
&: True → Both sides must be True
|: True → At least one side must be True
^: True → One side True and the other False

Memoization:
The dp array stores intermediate results to prevent redundant calculations, improving efficiency.

## My Java Solution


    
    class Solution {
    static int countWays(String s) {
        // code here
        int[][][] dp = new int[s.length()+1][s.length()+1][2];
        for(int i=0;i<dp.length;i++){
            for(int j=0;j<dp[i].length;j++) Arrays.fill(dp[i][j] , -1);
        }
        return helper(s , 0 , s.length()-1 , 1 , dp); 
    }
    static int helper(String s , int start , int end , int isTrue , int[][][] dp){
        if(start>end) return 0;
        if(start==end){
            if(isTrue==1){
                if(s.charAt(start)=='T') return 1;
                return 0;
            }
            else{
                if(s.charAt(start)=='F') return 1;
                return 0;
            }
        }
        if(dp[start][end][isTrue]!=-1) return dp[start][end][isTrue];
        int ways =0;
        for(int i=start+1;i<=end-1;i+=2){
            
            int lf = helper(s,start,i-1,0,dp);
            int lt = helper(s,start,i-1,1,dp);
            int rf = helper(s,i+1,end,0,dp);
            int rt = helper(s,i+1,end,1,dp);
            if(s.charAt(i)=='&'){
                if(isTrue==1){
                    ways+=(lt*rt);
                }
                else
                ways+=( lf*rf + lf*rt + lt*rf);
            }
            else if(s.charAt(i)=='|'){
                if(isTrue==1){
                    ways+=(lf*rt + lt*rt + lt*rf);
                }
                else
                ways+=(lf*rf);
            }
            else{
                if(isTrue==1) {
                    ways+=(rf*lt + rt*lf);
                }
                else
                ways+= (rt*lt + rf*lf);
            }
            
            // return ways;
        }
        return dp[start][end][isTrue] = ways;
    }}

  ## Complexity 
  Time Complexity:
The recursion has O(n³) complexity because for every operator, two recursive calls are made, leading to cubic complexity.
Memoization reduces redundant calls.

Space Complexity:
O(n²) for storing the 3D dp array, where n is the length of the string.
