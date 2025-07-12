## Question Link-: https://codeforces.com/problemset/problem/1594/C


---

###  Approach Explanation 

1. **Check if all characters** in the string are already equal to `c`:

   * If yes, **0 operations** are needed.

2. If not:

   * Try every `x` from `1` to `n`.
   * For each `x`, check whether **all positions divisible by `x`** already have character `c`.
   * If such an `x` is found, then **1 operation** using that `x` is sufficient.

3. If no such `x` works:

   * Use **2 operations** with `x = n-1` and `x = n` (this combination is always guaranteed to work).

---

###  **Time Complexity**

* **Worst-case per test case**:
  O(nlog n) — trying all `x` from `1` to `n`, and for each `x`, iterating through n/x positions.


---


```java
import java.util.*;
class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();
        for(int ind = 0;ind<t;ind++){
            
            int n = sc.nextInt();
            char c = sc.next().charAt(0);
            String s = sc.next();
            
            boolean same = true;
            for(int i=0;i<n;i++){
                if(s.charAt(i)!=c){
                    same = false;
                    break;
                }
            }
            if(same){
                System.out.println(0);
                continue;
            }
            
            int x = 1;
            int best = -1;
            for(int i=1;i<=n;i++){
                boolean flag = true;
                for(int j=i;j<=n;j=j+i){
                    if(s.charAt(j-1)!=c){
                        flag = false;
                        break;
                    }
                }
                if(flag){
                    best = i;
                    break;
                }
            }
            if(best!=-1){
                System.out.println(1);
                System.out.println(best);
            }
            else{
                System.out.println(2);
                System.out.println(n-1+" "+n);
            }
        }
    }
}
```

