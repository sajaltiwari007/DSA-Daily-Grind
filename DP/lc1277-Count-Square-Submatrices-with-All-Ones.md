## QuesLink-: https://www.geeksforgeeks.org/problems/count-square-submatrices-with-all-ones/1

Import question to be remembered.

## my java solution


        
        class Solution {
    public int countSquares(int n, int m, int mat[][]) {
        // Code here
        int[][] dp = new int[n][m];
        for(int i=0;i<n;i++){
            dp[i][0] = mat[i][0];
        }
        for(int i=0;i<m;i++){
            dp[0][i] = mat[0][i];
        }
        for(int i=1;i<n;i++){
            for(int j=1;j<m;j++){
                if(mat[i][j]!=0)
                dp[i][j] = 1 + Math.min(dp[i-1][j],Math.min(dp[i][j-1],dp[i-1][j-1]));
            }
        }
        
        // dp[i][j] = number of square matrices ending at i,j
        
        int sum = 0;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++) sum+=dp[i][j];
        }
        
        return sum;
        
        
    }}
