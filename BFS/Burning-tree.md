## QuesLink-: https://www.geeksforgeeks.org/problems/burning-tree/1

## Standard Interview Question.
## Key Concept is how to trace the parent node. use a hashmap to track the parent node. Apply standard bfs from target node.

## Java Solution-:

   
    class Solution {
    static Node start = null;
    public static int minTime(Node root, int target) {
        // code here
        HashMap<Node,Node> map = new HashMap<>();
        // map.put(root,null,target);
        inorder(root,map,target);
        Queue<Node> q = new LinkedList<>();
        q.offer(start);
        HashSet<Integer> set = new HashSet<>();
        set.add(start.data);
        int answer = 0;
        while(!q.isEmpty()){
            // answer++;
            int size = q.size();
            for(int i=1;i<=size;i++){
                Node node = q.poll();
                if(node.left!=null && !set.contains(node.left.data)){
                    set.add(node.left.data);
                    q.offer(node.left);
                }
                if(node.right!=null && !set.contains(node.right.data)){
                    set.add(node.right.data);
                    q.offer(node.right);
                }
                if(map.containsKey(node)){
                    Node parent = map.get(node);
                    if(!set.contains(parent.data)){
                        q.offer(parent);
                        set.add(parent.data);
                    }
                }
            }
            if(q.size()!=0) answer++;
            
        }
        return answer;
        
        
    }
    static void inorder(Node node,HashMap<Node,Node> map,int tarr){
        if(node==null) return;
        if(node.data==tarr){
            start = node;
        }
        if(node.left!=null) map.put(node.left,node);
        if(node.right!=null) map.put(node.right,node);
        inorder(node.left,map,tarr);
        inorder(node.right,map,tarr);
    }}
