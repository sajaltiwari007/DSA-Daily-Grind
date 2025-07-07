## QuesLink-: https://www.geeksforgeeks.org/problems/maximum-xor-of-two-numbers-in-an-array/1

Apprach-: Xor of numbers using Trie.

## Solution



    class Solution {
    static class Node {
        Node[] arr = new Node[2];
    }

    static Node root;

    public int maxXor(int[] arr) {
        root = new Node();
        insert(arr[0]);  
        int answer = 0;
        for (int i = 1; i < arr.length; i++) {
            answer = Math.max(answer, getXorAndInsert(arr[i]));
        }
        return answer;
    }
    static int getXorAndInsert(int x) {
        Node curr = root;
        int maxXor = 0;

        for (int i = 31; i >= 0; i--) {
            int bit = (x >> i) & 1;
            int opposite = bit ^ 1;

            if (curr.arr[opposite] != null) {
                maxXor |= (1 << i);
                curr = curr.arr[opposite];
            } else {
                curr = curr.arr[bit];
            }
        }

        curr = root;
        for (int i = 31; i >= 0; i--) {
            int bit = (x >> i) & 1;
            if (curr.arr[bit] == null) {
                curr.arr[bit] = new Node();
            }
            curr = curr.arr[bit];
        }

        return maxXor;
    }

    static void insert(int x) {
        Node curr = root;
        for (int i = 31; i >= 0; i--) {
            int bit = (x >> i) & 1;
            if (curr.arr[bit] == null) {
                curr.arr[bit] = new Node();
            }
            curr = curr.arr[bit];
        }
    }}
