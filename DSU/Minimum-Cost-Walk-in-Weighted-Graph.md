# Minimum Cost Walk in Weighted Graph. 
LeetCode -: https://leetcode.com/problems/minimum-cost-walk-in-weighted-graph/solutions/6559851/dcc-20-march-2025-dsu-by-sajaltiwari007-k8gm/

**Problem Statement:**
There is an undirected weighted graph with n vertices labeled from 0 to n - 1.

You are given the integer n and an array edges, where edges[i] = [ui, vi, wi] indicates that there is an edge between vertices ui and vi with a weight of wi.
A walk on a graph is a sequence of vertices and edges. The walk starts and ends with a vertex, and each edge connects the vertex that comes before it and the vertex that comes after it. It's important to note that a walk may visit the same edge or vertex more than once.
The cost of a walk starting at node u and ending at node v is defined as the bitwise AND of the weights of the edges traversed during the walk. In other words, if the sequence of edge weights encountered during the walk is w0, w1, w2, ..., wk, then the cost is calculated as w0 & w1 & w2 & ... & wk, where & denotes the bitwise AND operator.
You are also given a 2D array query, where query[i] = [si, ti]. For each query, you need to find the minimum cost of the walk starting at vertex si and ending at vertex ti. If there exists no such walk, the answer is -1.
Return the array answer, where answer[i] denotes the minimum cost of a walk for query i.

Example 1:

Input: n = 5, edges = [[0,1,7],[1,3,7],[1,2,1]], query = [[0,3],[3,4]]
Output: [1,-1]
Explanation:To achieve the cost of 1 in the first query, we need to move on the following edges: 0->1 (weight 7), 1->2 (weight 1), 2->1 (weight 1), 1->3 (weight 7).
In the second query, there is no walk between nodes 3 and 4, so the answer is -1.

**Solution:**  
```java
// Considering the Edge weights of all the components together.
class DSU{
    List<Integer> parent = new ArrayList<>();
    List<Integer> size = new ArrayList<>();// For Path Compression

    public DSU(int n){
        for(int i=0;i<n;i++){
            parent.add(i);
            size.add(1);
        }
    }

    public int find_Parent(int x){

        if(parent.get(x)==x) return x;

        int x_parent = find_Parent(parent.get(x));
        parent.set(x,x_parent);

        return parent.get(x);

    }

    public void take_union(int x , int y){

        int x_parent = find_Parent(x);
        int y_parent = find_Parent(y);

        if(x_parent==y_parent) return;

        if(size.get(x_parent)>size.get(y_parent)){
            parent.set(y_parent ,x_parent );
            size.set(x_parent , size.get(x_parent)+size.get(y_parent));
        }
        else{
            parent.set(x_parent ,y_parent );
            size.set(y_parent , size.get(x_parent)+size.get(y_parent));
        }


    }

    public List<Integer> getParentList() {
        return parent;
    }

}
class Solution {
    public int[] minimumCost(int n, int[][] edges, int[][] query) {

        DSU ds = new DSU(n);
        // int[] nums = new int[n];
        HashMap<Integer,List<Integer>> map2 = new HashMap<>();

        for(int[] i:edges){

            int x = i[0];
            int y = i[1];
            if(!map2.containsKey(x)) map2.put(x,new ArrayList<>());
            if(!map2.containsKey(y)) map2.put(y,new ArrayList<>());
            map2.get(x).add(i[2]);
            map2.get(y).add(i[2]);


            if(ds.find_Parent(x)!=ds.find_Parent(y)){
                ds.take_union(x , y);
            }
        }

        // HashSet<Integer> set = new HashSet<>();
        HashMap<Integer,Integer> map = new HashMap<>();
        // List<Integer> parent = ds.getParentList();
        // HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            int parent = ds.find_Parent(i);
            List<Integer> temp = map2.containsKey(i) ? (map2.get(i)) : new ArrayList<>();
            int and = -1;
            for(int j:temp) and = (and&j);
            map.put(parent, map.containsKey(parent) ? (map.get(parent) & and) : and);
        }
        

        int[] answer = new int[query.length];
        int ind=0;
        for(int[] i:query){

            if(ds.find_Parent(i[0])!=ds.find_Parent(i[1])){
                answer[ind++]=-1;
            }
            else
            answer[ind++] = map.get(ds.find_Parent(i[0]));
        }
        return answer;

        
    }
}

