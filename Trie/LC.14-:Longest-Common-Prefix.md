## https://leetcode.com/problems/longest-common-prefix/description/

Trie Basic Implementation from interview pov.
## Java solution


    class Solution {
    class Node{
        boolean has;
        Node[] arr;
        Node(){
            this.has = false;
            arr = new Node[26];
        }
    }
    Node root;
    String answer = "";
    public String longestCommonPrefix(String[] strs) {
          int n = strs.length;
         if(n==1) return strs[0];
         root = new Node();
         answer = strs[0];
         insert(strs[0]);
         for(int i=1;i<strs.length;i++){

             StringBuilder sb = new StringBuilder();
             findPrefix(strs[i],sb);
             if(sb.length()<answer.length()){
                answer = sb.toString();
             }
         }
         return answer;
         
        
    }
    void findPrefix(String s,StringBuilder sb){
        Node curr = root;
        for(int i=0;i<s.length();i++){
            int x = (int)s.charAt(i)-97;
            if(curr.arr[x]!=null){
                sb.append(s.charAt(i)+"");
                curr = curr.arr[x];
            }
            else{
                break;
            }
        }
        curr.has = true;
    }
    void insert(String s){
        Node curr = root;
        for(int i=0;i<s.length();i++){
            int x = (int)s.charAt(i)-97;
            if(curr.arr[x]==null) curr.arr[x] = new Node();
            curr = curr.arr[x];
        }
        curr.has = true;
    }}
