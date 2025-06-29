## QuesLink -: https://leetcode.com/problems/number-of-subsequences-that-satisfy-the-given-sum-condition/description/?envType=daily-question&envId=2025-06-29

## My Java Solution-:  Uses Two Pointers Approach 




    class Solution {
    static int mod = 1000000007;
    public int numSubseq(int[] nums, int target) {
        Arrays.sort(nums);
        int n = nums.length;
        int[] pow = new int[n];
        pow[0] = 1;
        for (int i = 1; i < n; i++) {
            pow[i] = (pow[i - 1] * 2) % mod;
        }

        long ans = 0;
        int start = 0, end = n - 1;

        while (start <= end) {
            if (nums[start] + nums[end] <= target) {
                ans = (ans + pow[end - start]) % mod;
                start++;
            } else {
                end--;
            }
        }

        return (int) ans;
    }}
