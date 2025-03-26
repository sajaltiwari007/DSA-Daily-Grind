## Problem link-:
https://www.codechef.com/problems/UNLOCKSAFE?tab=statement 

### **Explanation**:
1. **Greedy Choice**:  
    - The code calculates the absolute difference between the corresponding elements of arrays `a` and `b`.
    - It minimizes the difference using the fact that numbers are on a circular scale (0 to 9), allowing two options: 
      - Moving forward (`diff`)
      - Moving backward (`9 - diff`)
    - The smaller of these two values is used as the optimal move.

2. **State Maintenance**:  
    - It tracks the remaining moves using the variable `totalMoves`.
    - If at any point `totalMoves` becomes negative, it returns **"NO"** since further moves are impossible.

3. **Parity Check**:
    - After using the minimum moves, it checks if the remaining moves are even or zero, as even moves can always result in a balanced state.
    - If moves are odd, it checks if any difference (from `maxDiff[]`) is odd and can be used to balance the moves.

### **Algorithm Topic**:  
- **Greedy Algorithm**  
- **Modulo and Circular Number Properties**  
- **Mathematical Problem Solving**

## Complexity
### **Time Complexity**:  
- **Reading Input and Initial Setup**: O(n)
- **Calculating Differences and Moves**: O(n) 
  - For each element, the absolute difference and the optimal move using `Math.min(diff, 9 - diff)` is computed in constant time.
- **Checking Remaining Moves**: \( O(n) \)  
  - After the moves, the code checks the `maxDiff[]` array to determine if further moves can balance the state.

Overall, for each test case: 
O(n)

Since there are **t** test cases:  
O(t.n)

---

### **Space Complexity**:  
- Arrays `a`, `b`, and `maxDiff` all require ( O(n) ) space.
- Constant space for variables like `diff`, `totalMoves`, etc.

Overall: 
O(n)

Hence, the final complexity is:
- **Time Complexity**: O(t.n)
- **Space Complexity**: O(n)

## My Java Solution

  
        import java.util.*;
        class Codechef {
    public static void main(String[] args) throws java.lang.Exception {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();while (t-- > 0) {
            int n = sc.nextInt();
            long k = sc.nextLong();
            
            int[] a = new int[n];
            int[] b = new int[n];
            
            for (int i = 0; i < n; i++) a[i] = sc.nextInt();
            for (int i = 0; i < n; i++) b[i] = sc.nextInt();
            
            boolean result = canTransform(a, b, n, k);
            System.out.println(result ? "YES" : "NO");
        }
    }

    static boolean canTransform(int[] a, int[] b, int n, long totalMoves) {
        long[] maxDiff = new long[n];

        for (int i = 0; i < n; i++) {
            int diff = Math.abs(a[i] - b[i]);
            totalMoves -= Math.min(diff, 9 - diff);
            if (totalMoves < 0) return false;
            maxDiff[i] = Math.max(diff, 9 - diff) - Math.min(diff, 9 - diff);
        }
        
        if (totalMoves == 0 || totalMoves % 2 == 0) return true;
        
        for (long diff : maxDiff) {
            if (diff % 2 != 0 && diff <= totalMoves) {
                return true;
            }
        }
        
        return false;
    }}
