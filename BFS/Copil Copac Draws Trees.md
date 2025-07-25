## Link-: https://codeforces.com/contest/1830/problem/A
---

Here is a properly formatted `README.md` file for your GitHub repository, explaining the approach, how `val` is used to track readings, and the time complexity:

---

#  Copil Copac Tree Drawing Problem

This repository contains the Java solution to the **Copil Copac Tree Drawing** problem from competitive programming. It efficiently calculates the number of "readings" required to draw a tree based on edge input order.

##  Problem Summary

Copil Copac draws a tree using the following algorithm:

- **Step 0:** Draw vertex 1.
- **Step 1:** Iterate over the edges **in input order**. If an edge connects a drawn node to an undrawn node, draw that undrawn node and the edge.
- **Step 2:** If all vertices are drawn, terminate. Otherwise, return to step 1.

###  Task

Given a tree with `n` vertices and `n - 1` edges (in input order), compute the number of readings (passes through the edge list) Copil needs to complete the drawing.

---

## Approach

We simulate the process using a **Breadth-First Search (BFS)** traversal. But unlike standard BFS (which processes nodes in arbitrary neighbor order), we respect the **input edge order** by associating each edge with its index.

We define a `Pair` class:
```java
static class Pair {
    int node;   // Node number
    int ind;    // Index of the edge used to reach this node
    int val;    // Current reading count at this node
}
````

### 🔄 BFS Simulation

* Start from vertex `0` (1-based index becomes 0).
* Enqueue `Pair(0, -1, 1)` → node 0, no previous edge, initial reading = 1.
* For each neighbor:

  * If its edge index is **greater than or equal** to the previous edge index → continue in the **same reading**.
  * If edge index is **less** than the previous edge → implies the input list has "wrapped" → increment reading.

This logic mimics the behavior of reading the edge list linearly.

---

##  How `val` Tracks Readings

* `val` represents the **current reading number** during BFS traversal.
* It increases **only when** the input edge order is violated (i.e., we must loop over the edge list again).
* So, when we jump to a lower-indexed edge, it indicates a **new reading** is needed.

Example logic:

```java
if (currentEdgeIndex > previousEdgeIndex) {
    // Continue in the same reading
    q.offer(new Pair(nextNode, currentEdgeIndex, val));
} else {
    // Jumped to an earlier edge in input — increment reading count
    q.offer(new Pair(nextNode, currentEdgeIndex, val + 1));
}
```

We track the **maximum `val`** reached — this is our final answer.

---

##  Time Complexity

Let `n` be the number of nodes.

* **Reading input:** O(n)
* **Building adjacency list:** O(n)
* **BFS traversal:** Each node and edge is visited once → O(n)
* **Total per test case:** **O(n)**
* Across all test cases: sum of `n` ≤ 2×10⁵ → overall **efficient**


---
## Java Solution
```
import java.util.*;
public class Main {
  static class Pair{
    int node;
    int ind;
    int val;
    Pair(int n,int i,int v){
      this.node = n;
      this.ind = i;
      this.val = v;
    }
  }
  public static void main(String[] args) {
    
    Scanner sc = new Scanner(System.in);
    int t = sc.nextInt();

    while(t-->0){

      int n = sc.nextInt();
      int[][] arr = new int[n-1][2];
      for(int i=0;i<arr.length;i++){
        arr[i][0] = sc.nextInt();
        arr[i][1] = sc.nextInt();
        arr[i][0]-=1;
        arr[i][1]-=1;
      }

      List<List<Pair>> adj = new ArrayList<>();
      for(int i=0;i<n;i++) adj.add(new ArrayList<>());
      for(int i=0;i<arr.length;i++){
          adj.get(arr[i][0]).add(new Pair(arr[i][1],i,-1));
          adj.get(arr[i][1]).add(new Pair(arr[i][0],i,-1));
      }

      Queue<Pair> q = new LinkedList<>();
      boolean[] visited = new boolean[n];
      q.offer(new Pair(0,-1,1));
      visited[0] = true;
      int answer = 0;
      while(!q.isEmpty()){

        int size = q.size();
        for(int j=0;j<size;j++){
        Pair p = q.poll();
        int node = p.node;
        int ind = p.ind;
        int val = p.val;
        answer = Math.max(answer,val);
        for(Pair i:adj.get(node)){
          if(visited[i.node]==true) continue;
            if(i.ind>ind){
              visited[i.node] = true;
              q.offer(new Pair(i.node,i.ind,val));
            }
            else{
              visited[i.node] = true;
              q.offer(new Pair(i.node,i.ind,val+1));
            }
        }}
        
      } 
      System.out.println(answer);

      
      
    }
    
  }
}

