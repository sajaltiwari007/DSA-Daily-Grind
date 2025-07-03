## QuesLink-: https://www.geeksforgeeks.org/problems/count-of-distinct-substrings/1

## Question Description
Given a string of length N of lowercase alphabet characters. The task is to complete the function countDistinctSubstring(), which returns the count of total number of distinct substrings of this string.
Constraints:
1 ≤ T ≤ 10
1 ≤ N ≤ 1000

Example(To be used only for expected output):
Input:
2
ab
ababa

Output:
4
10

## Java Solution using Trie Concepts.( Can also be solved using hashset but will take higher complexity.)

    
    class GfG {
    static class Node{
    
    Node[] arr;
    Node(){
        arr = new Node[26];
     }
    }
    
    static Node root;
    
    public static int countDistinctSubstring(String s) {
        
        root = new Node();
        int answer = 0;
        for(int i=0;i<s.length();i++){
            Node curr = root;
            for(int j=i;j<s.length();j++){
                int x = (int)s.charAt(j)-97;
                if(curr.arr[x]==null){
                    curr.arr[x] = new Node();
                    answer++;
                }
                curr = curr.arr[x];
            }
        }
        
        return answer+1;
        
    }}
