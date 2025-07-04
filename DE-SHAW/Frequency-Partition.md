## Ques Description-:
You are given an array of N integers arr and another integer k. You need to partition arr into consecutive subarrays such that every element of arr is a part of exactly one subarray and the count of numbers with odd frequency in every subarray doesn't exceed k. Find the number of ways to partition arr while satisfying the given conditions. As the answer can be very large, print it modulo by 10^9+7
## Approach-: Partition DP

## Solution



    import java.util.*;
    public class Main {
    static final int MOD = 1000000007;static int n, k;
    static long[] arr;
    static long[] mem;
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();

        while (t-- > 0) {
            n = sc.nextInt();
            k = sc.nextInt();

            arr = new long[n];
            for (int i = 0; i < n; i++) {
                arr[i] = sc.nextLong();
            }

            mem = new long[n + 1];
            Arrays.fill(mem, -1);

            System.out.println(dp(0));
        }

        sc.close();
    }

    static long dp(int id) {
        if (id == n) return 1;

        if (mem[id] != -1) return mem[id];

        long ans = 0;
        int oddCount = 0;
        Map<Long, Integer> freq = new HashMap<>();

        for (int i = id; i < n; i++) {
            long cur = arr[i];
            freq.put(cur, freq.getOrDefault(cur, 0) + 1);

            // update odd count
            if (freq.get(cur) % 2 == 1) oddCount++;
            else oddCount--;

            if (oddCount <= k) {
                ans = (ans + dp(i + 1)) % MOD;
            }
        }

        mem[id] = ans;
        return ans;
    }}
