## Ques Description-: 
You are given an array of N integers arr and another integer r. You need to choose three indices (i,j,k) such that i<=j<=k and the value of the expression ( arr[i]+arr[j]*r+arr[k]*r*r ) is maximized. Find the maxium value.
Constraint -: N upto 10^5 , |r|<=100 , |arr[x]|<=10^9

## Strategy
We fix each j and:
Track the maximum arr[i] for i <= j
Track the maximum arr[k] for k >= j
Precompute best left and right values and for every j computer the value of the expression

## Java Solution



     import java.util.*;
     public class Main {
     public static void main(String[] args) {  
        Scanner sc = new Scanner(System.in);
        int T = sc.nextInt();
          while (T-- > 0) {
            int N = sc.nextInt();
            int r = sc.nextInt();
            int[] arr = new int[N];

            for (int i = 0; i < N; i++) {
                arr[i] = sc.nextInt();
            }

            // Precompute left max and right max
            long[] leftMax = new long[N];
            long[] rightMax = new long[N];

            leftMax[0] = arr[0];
            for (int i = 1; i < N; i++) {
                leftMax[i] = Math.max(leftMax[i - 1], arr[i]);
            }

            rightMax[N - 1] = arr[N - 1];
            for (int i = N - 2; i >= 0; i--) {
                rightMax[i] = Math.max(rightMax[i + 1], arr[i]);
            }

            long maxValue = Long.MIN_VALUE;
            for (int j = 0; j < N; j++) {
                long value = leftMax[j] + (long) arr[j] * r + (long) rightMax[j] * r * r;
                maxValue = Math.max(maxValue, value);
            }

            System.out.println(maxValue);
        }

        sc.close();
    }}
