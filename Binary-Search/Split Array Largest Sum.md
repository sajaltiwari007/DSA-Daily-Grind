## QuesLink-: https://www.geeksforgeeks.org/problems/split-array-largest-sum--141634/1

## My Java Solution


        class Solution {
    public int splitArray(int[] arr, int k) {
        long start = Long.MIN_VALUE;
        long end = 0;
        for (int i : arr) {
            end += (long) i;
            start = Math.max(start, i);
        }
        long answer = end;

        while (start <= end) {
            long mid = start + (end - start) / 2;
            int x = helper(arr, mid);
            if (x <= k) {
                answer = mid; 
                end = mid - 1;
            } else {
                start = mid + 1; 
            }
        }

        return (int) answer;
    }

    static int helper(int[] arr, long maxSum) {
        long curr = 0;
        int cnt = 1; 

        for (int i : arr) {
            if (i > maxSum) return Integer.MAX_VALUE; 

            if (curr + i <= maxSum) {
                curr += i;
            } else {
                cnt++;      
                curr = i;  
            }
        }

        return cnt;
    }}
