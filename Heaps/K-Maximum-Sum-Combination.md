## https://www.geeksforgeeks.org/problems/maximum-sum-combination/1
Main Concept-: Need to understand how to generate K max sum using all the possible pairs in less that quadratric complexity. Quite similar to BFS appraoch.

## Java Solution-: 

    class Solution {
    static class Pair{
        int sum;
        int x;
        int y;
        Pair(int sum,int x,int y){
            this.sum = sum;
            this.x = x;
            this.y = y;
        }
    }
    public ArrayList<Integer> topKSumPairs(int[] a, int[] b, int k) {
        Arrays.sort(a);
        Arrays.sort(b);
        ArrayList<Integer> answer = new ArrayList<>();
        HashSet<String> visited = new HashSet<>();
        PriorityQueue<Pair> q = new PriorityQueue<>((aa,bb)->Integer.compare(bb.sum,aa.sum));
        int i = a.length-1;
        int j = b.length-1;
        q.offer(new Pair(a[i]+b[j],i,j));
        String s = String.valueOf(i)+"#"+String.valueOf(j);
        visited.add(s);
        while(answer.size()<k){
            
            Pair p = q.poll();
            answer.add(p.sum);
            int row = p.x;
            int col = p.y;
            if(row-1>=0){
                s = String.valueOf(row-1)+"#"+String.valueOf(col);
                if(!visited.contains(s)){
                    visited.add(s);
                    q.offer(new Pair(a[row-1]+b[col],row-1,col));
                }
                
            }
            if(col-1>=0){
                s = String.valueOf(row)+"#"+String.valueOf(col-1);
                if(!visited.contains(s)){
                    visited.add(s);
                    q.offer(new Pair(a[row]+b[col-1],row,col-1));
                }
                
            }
        }
        return answer;
        
    }}
