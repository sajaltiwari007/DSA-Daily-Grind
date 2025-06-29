## QuesLink-: https://www.geeksforgeeks.org/problems/longest-string-chain/1
## Problem Description -: You are given an array of words where each word consists of lowercase English letters.

wordA is a predecessor of wordB if and only if we can insert exactly one letter anywhere in wordA without changing the order of the other characters to make it equal to wordB.

For example, "abc" is a predecessor of "abac", while "cba" is not a predecessor of "bcad".
A word chain is a sequence of words [word1, word2, ..., wordk] with k >= 1, where word1 is a predecessor of word2, word2 is a predecessor of word3, and so on. A single word is trivially a word chain with k == 1.

Return the length of the longest possible word chain with words chosen from the given list of words.

Constraints-: 1 <= words.length <= 10^4
1 <= words[i].length <= 10
 words[i] only consists of lowercase English letters.

 ## Solution 1-:Java Solution Using LIS(DP)


          class Solution {
    public int longestStrChain(String[] words) {
    Arrays.sort(words,(a,b)->Integer.compare(a.length(),b.length()));
        
        int n = words.length;
        int[] dp = new int[n];
        Arrays.fill(dp,1);
        
        for(int i=1;i<n;i++){
            for(int j=0;j<i;j++){
                if(words[j].length()+1==words[i].length() && helper(words[j],words[i])){
                    dp[i] = Math.max(dp[i],1+dp[j]);
                }
            }
        }
        // System.out.println(Arrays.toString(dp));
        int answer = 0;
        for(int i:dp) answer = Math.max(i,answer);
        return answer;
    }
    static boolean helper(String a,String b){
        int i=0;
        int j=0;
        boolean flag = true;
        while(i<a.length() && j<b.length()){
            if(a.charAt(i)==b.charAt(j)){
                i++;
                j++;
            }
            else if(flag){
                j++;
                flag = false;
            }
            else return false;
        }
        
        return (i>=a.length() && j>=b.length() && !flag) ||(i>=a.length() && j==b.length()-1 && flag);
        
    }}

  ##  Solution 2-: Uses the Concept of Hashing for better optimization

 
        
         class Solution {
    public int longestStringChain(String words[]) {
       Arrays.sort(words,(a,b)->Integer.compare(a.length(),b.length()));
       HashMap<String,Integer> map = new HashMap<>();
        int answer = 0;
        
        for(int i=0;i<words.length;i++){
            String s = words[i];
            map.put(s,1);
            for(int j=0;j<s.length();j++){
                String find = s.substring(0,j)+s.substring(j+1);
                if(map.containsKey(find)){
                    map.put(s,Math.max(map.get(s),map.get(find)+1));
                }
            }
            answer = Math.max(answer,map.get(s));
        }
        return answer;
    }}

