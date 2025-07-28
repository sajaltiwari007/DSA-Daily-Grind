## Ques Link-: https://www.hackerrank.com/challenges/cut-the-tree/problem?isFullScreen=true

#### Problem Summary

You are given a tree with `n` nodes and `n - 1` edges. Each node has an associated integer value (given in the `data` list). The objective is to remove exactly one edge in the tree such that the absolute difference between the sum of node values in the resulting two trees is minimized.

This is a classical tree partitioning problem. Since it is a tree, removing any edge will always split it into exactly two connected components.

---

### Approach

1. **Construct the Tree**:

   * Represent the tree using an adjacency list `adj` from the list of edges. Since the tree is undirected, each edge is added in both directions.

2. **Compute Total Tree Sum**:

   * Calculate the total sum of all node values. This is needed to determine the value of the other component when one subtree is removed.

3. **Run DFS Traversal**:

   * Use Depth-First Search starting from node 0 (or any arbitrary root).
   * In the DFS, compute the sum of the subtree rooted at each node.
   * While backtracking in DFS, simulate the removal of the edge connecting the current node to its parent. The resulting two trees will have sums: `childSum` and `totalSum - childSum`.
   * Compute the absolute difference between these two sums and update the minimum value (tracked in `answer`).

4. **Return Minimum Difference**:

   * After traversing the entire tree and evaluating all edge removals, return the smallest difference found.

---

### Detailed Intuition Behind `dfs` Function

```java
private static int dfs(int node, List<List<Integer>> adj, List<Integer> data, boolean[] visited)
```

#### What it does:

* Performs a **post-order DFS traversal** to compute the sum of the subtree rooted at `node`.
* Recursively visits all unvisited neighbors (children) of the current node.
* For each subtree rooted at a child node:

  * Simulates cutting the edge between `node` and that child.
  * Calculates the **difference** between the two components after the cut.
  * Updates the `answer` if the new difference is smaller.
* Returns the **total sum of the subtree** rooted at `node`.

#### Line-by-line breakdown:

```java
visited[node] = true;
int all = data.get(node);
```

* Mark the current node as visited.
* Initialize `all` with the value of the current node (this will be updated to include subtree sums).

```java
for(int i : adj.get(node)){
    if(!visited[i]){
        int childSum = dfs(i, adj, data, visited);
        all += childSum;
        answer = Math.min(answer, Math.abs(totalSum - 2 * childSum));
    }
}
```

* For each unvisited child:

  * Compute its subtree sum via recursion.
  * Add that sum to the current node’s cumulative subtree sum.
  * Simulate removing the edge between this node and the child:

    * One part has sum = `childSum`
    * Other part has sum = `totalSum - childSum`
    * Difference = `|totalSum - 2 * childSum|`
  * Update the minimum difference found so far.

```java
return all;
```

* Return the total subtree sum for the current node to its parent in the recursion.

---

### Time and Space Complexity Analysis

#### Time Complexity: **O(n)**

* Each node is visited exactly once in the DFS traversal.
* Each edge is traversed once in both directions (undirected graph), leading to a total of `2(n - 1)` operations, which is still linear.
* Total sum computation is also O(n).

So the overall time complexity is **linear in the number of nodes**, that is, **O(n)**.

#### Space Complexity: **O(n)**

* Adjacency list takes O(n) space.
* The recursion stack in the worst case (if the tree is skewed) is O(n).
* The `visited` array also takes O(n) space.

So the space complexity is **O(n)**.

---


```
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.function.*;
import java.util.regex.*;
import java.util.stream.*;
import static java.util.stream.Collectors.joining;
import static java.util.stream.Collectors.toList;

class Result {
    
     static int totalSum = 0;
    static int answer = Integer.MAX_VALUE;

    public static int cutTheTree(List<Integer> data, List<List<Integer>> edges) {
        int n = data.size();

        // Build adjacency list
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) adj.add(new ArrayList<>());
        for (List<Integer> edge : edges) {
            int u = edge.get(0) - 1; // Convert to 0-based
            int v = edge.get(1) - 1;
            adj.get(u).add(v);
            adj.get(v).add(u);
        }

        totalSum = data.stream().mapToInt(i -> i).sum();

        boolean[] visited = new boolean[n];
        dfs(0, adj, data, visited);

        return answer;
    }

    private static int dfs(int node, List<List<Integer>> adj, List<Integer> data, boolean[] visited) {
      
        visited[node] = true;
        int all = data.get(node);
        
        for(int i:adj.get(node)){
          
          if(!visited[i]){
            int childSum = dfs(i,adj,data,visited);
            all+=childSum;
            answer = Math.min(answer,Math.abs(totalSum-2*childSum));
            
          }
        }
      return all;
      
        

    
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<Integer> data = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        List<List<Integer>> edges = new ArrayList<>();

        IntStream.range(0, n - 1).forEach(i -> {
            try {
                edges.add(
                    Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
                        .map(Integer::parseInt)
                        .collect(toList())
                );
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        });

        int result = Result.cutTheTree(data, edges);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
