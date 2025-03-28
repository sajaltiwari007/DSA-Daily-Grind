## Problem Link 
https://leetcode.com/problems/maximum-number-of-points-from-grid-queries/description/

## Problem Statment
You are given an m x n integer matrix grid and an array queries of size k.
Find an array answer of size k such that for each integer queries[i] you start in the top left cell of the matrix and repeat the following process:
If queries[i] is strictly greater than the value of the current cell that you are in, then you get one point if it is your first time visiting this cell, and you can move to any adjacent cell in all 4 directions: up, down, left, and right.
Otherwise, you do not get any points, and you end this process.
After the process, answer[i] is the maximum number of points you can get. Note that for each query you are allowed to visit the same cell multiple times.
Return the resulting array answer.
Example 1:
Input: grid = [[1,2,3],[2,5,7],[3,5,1]], queries = [5,6,2]
Output: [5,8,1]
Explanation: The diagrams above show which cells we visit to get points for each query.

## Appraoch
This problem requires us to determine the maximum number of points accessible in a grid where movement is constrained by the values in the grid cells. The approach uses a Priority Queue (Min-Heap) + BFS (Dijkstra-like Traversal).
Key Observations
Grid Exploration with Priority Queue
We start from (0,0), inserting it into a priority queue with an adjusted value (grid[0][0] + 1).
We explore the four adjacent cells (up, down, left, right), updating their values appropriately in the queue.
The key idea is that a cell (i, j) can only be reached if its value is less than the value of the current cell being processed.
Prefix Sum to Answer Queries Efficiently
The create[] array records the number of cells accessible with a given minimum value.
We convert create[] into a prefix sum array so that for each query, we can get the answer in O(1) time

## Algorithm
Initialize Priority Queue (Min-Heap)
Insert (0,0) into the heap with value grid[0][0] + 1.
Maintain a visited[][] array.
Perform BFS with Priority Queue
Extract the smallest element (i, j, val) from the queue.
If (i, j) is already visited, skip it.
Mark (i, j) as visited and increment create[val].
Traverse its four adjacent cells:
If the next cell value is less than val, push (row, col, val).
Otherwise, push (row, col, grid[row][col] + 1).
Compute Prefix Sum for create[]
Convert create[] into a prefix sum array.
Answer Queries in O(1)
For each query arr[i], return create[arr[i]].

## Complexity Analysis

Grid Traversal:
Each cell is processed once, and every cell is pushed and popped from the heap once.
Time Complexity: 
O(nmlog(nm)) (Dijkstra-like approach).
Prefix Sum Construction:
Constructing create[] takes  O(N).
Query Answering:
Each query is answered in O(1).

Overall Complexity: O(nmlog(nm)+Q) where Q is the number of queries.

## My Java Solution



    class Solution {
    class Pair{
        int i;
        int j;
        int val;
        Pair(int i,int j,int v){
            this.i=i;
            this.j=j;
            this.val=v;
        }
    }
    public int[] maxPoints(int[][] grid, int[] queries) {
        int n = queries.length;
        int[] answer = new int[n];
        helper(grid , queries , answer);
        return answer;
    }
    void helper(int[][] grid , int[] arr  , int[] answer){
        int n = grid.length;
        int m = grid[0].length;
        boolean[][] visited = new boolean[n][m];

        PriorityQueue<Pair> q = new PriorityQueue<>((a,b)->Integer.compare(a.val,b.val));
        q.offer(new Pair(0,0,grid[0][0]+1));
        // visited[0][0]=true;

        int[] create = new int[1000000+2];
        while(!q.isEmpty()){

            Pair p = q.poll();
            int i = p.i;
            int j = p.j;
            int val = p.val;
            // System.out.println(i +" "+j+" "+visited[i][j]);
            if(visited[i][j]) continue;
            visited[i][j]=true;
            create[val]+=1;
            int[] r = {0,0,1,-1};
            int[] c = {1,-1,0,0};

            for(int ind=0;ind<4;ind++){

                int row = i+r[ind];
                int col = j+c[ind];
                if(row>=0 && row<n && col>=0 && col<m && !visited[row][col]){
                    if(grid[row][col]<val){
                        q.offer(new Pair(row,col,val));
                        //  create[val]+=1;
                        // visited[row][col]=true;
                    }
                    else{
                           q.offer(new Pair(row,col,grid[row][col]+1));
                    }
                }

            }
        }
        // System.out.println(Arrays.toString(create));
        construct(create,answer,arr);
    }

    void construct(int[] create , int[] answer, int[] arr){

        for(int i=1;i<create.length;i++){
            create[i]+=create[i-1];
        }

        for(int i=0;i<answer.length;i++){
            answer[i] = create[arr[i]];
        }
    }}
