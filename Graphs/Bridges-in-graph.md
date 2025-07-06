## Question Link-: https://leetcode.com/problems/critical-connections-in-a-network/

## Brute Force-: Using DSU Complexiyt-: O(E^2*constant)
## Brute Solution-: 



    class DSU{
    List<Integer> size = new ArrayList<>();
    List<Integer> parent = new ArrayList<>();
    DSU(int n){
    for(int i=0;i<n;i++){
            size.add(1);
            parent.add(i);
        }
    }
    List<Integer> get(){
        return parent;
    }
    int find_parent(int x){

        if(x==parent.get(x)) return x;
        int up = find_parent(parent.get(x));
        parent.set(x,up);
        return up;
    }

    void union(int x,int y){

        int ux = find_parent(x);
        int uy = find_parent(y);
        if(ux==uy) return;

        if(size.get(ux)>size.get(uy)){
            parent.set(uy,ux);
            size.set(ux,size.get(ux)+size.get(uy));
        }
        else if(size.get(ux)<=size.get(uy)){
            parent.set(ux,uy);
            size.set(uy,size.get(ux)+size.get(uy));
        }
  

    }}
   
    class Solution {
    boolean isConnected(int n,List<List<Integer>> adj,List<Integer> list){

        DSU ds = new DSU(n);
        for(List<Integer> temp :adj){

            if(temp.get(0)==list.get(0) && temp.get(1)==list.get(1)) continue;
            ds.union(temp.get(0),temp.get(1));
        }
        List<Integer> parent = ds.get();
        HashSet<Integer> set = new HashSet<>();
        for(int i:parent) set.add(ds.find_parent(i));

        return set.size()==1;

        
        

    }


    public List<List<Integer>> criticalConnections(int n, List<List<Integer>> connections) {

        List<List<Integer>> answer = new ArrayList<>();

        for(List<Integer> list:connections){
            
            if(!isConnected(n,connections,list)){
                answer.add(new ArrayList<>(list));
            }
        }
        return answer;
        
    }}

## Optimal Appraoch -: using Tarjan's Algorithm.
## Refer-: https://www.youtube.com/watch?v=qrAub5z8FeA&t=777s
## Optimal Solution using DFS



     class Solution {
    static int time = 1;
    public List<List<Integer>> criticalConnections(int n, List<List<Integer>> connections) {
    List<List<Integer>> adj = new ArrayList<>();
        for(int i=0;i<n;i++) adj.add(new ArrayList<>());
        for(List<Integer> list:connections){
            adj.get(list.get(0)).add(list.get(1));
            adj.get(list.get(1)).add(list.get(0));
        }
        int[] entry = new int[n];
        int[] low = new int[n];
        boolean[] visited = new boolean[n];
        List<List<Integer>> bridge = new ArrayList<>();
        dfs(adj,entry,low,bridge,0,-1,visited);
        return bridge;
        
    }
    void dfs(List<List<Integer>> adj , int[] entry, int[] low,  List<List<Integer>> bridge, int node, int parent , boolean[] visited){
        visited[node] = true;
        entry[node]=time;
        low[node]=time;
        time++;
        for(int i:adj.get(node)){

            if(i==parent) continue;
            if(!visited[i]) dfs(adj,entry,low,bridge,i,node,visited);
            
            low[node] = Math.min(low[node],low[i]);
            if(entry[node]<low[i]){
                bridge.add(Arrays.asList(i,node));
            }

                  
        }
    }}
