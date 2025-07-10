##  QuesLink-: https://www.geeksforgeeks.org/problems/find-the-longest-string--170645/1


###  Problem:

Find the longest string in the array such that all its prefixes are also present in the array.

---

###  Approach & Intuition:

We use a **Trie (Prefix Tree)** to store all strings.
Then, for each string, we **check if all its prefixes exist** in the Trie by verifying the `has` flag at every step.

If valid, we track the **longest such string**, and in case of a tie, choose the **lexicographically smaller** one.

---

###  Complexity:

* **Time:** `O(N * L)` where `N = number of words`, `L = average word length`
* **Space:** `O(N * L)` for the Trie structure

---

###  Code:

```java
class Node {
    boolean has;
    Node[] arr;
    Node() {
        arr = new Node[26];
    }
}

class Solution {
    static Node root;

    public String longestString(String[] arr) {
        root = new Node();
        for (int i = 0; i < arr.length; i++) {
            insert(arr[i]);
        }

        String answer = "";
        for (int i = 0; i < arr.length; i++) {
            if (exist(arr[i])) {
                if (answer.length() < arr[i].length()) answer = arr[i];
                else if (answer.length() == arr[i].length()) {
                    if (answer.compareTo(arr[i]) > 0) answer = arr[i];
                }
            }
        }
        return answer;
    }

    public boolean exist(String s) {
        Node curr = root;
        for (int i = 0; i < s.length(); i++) {
            int x = s.charAt(i) - 'a';
            if (curr.arr[x] == null) return false;
            curr = curr.arr[x];
            if (!curr.has) return false;
        }
        return true;
    }

    static void insert(String s) {
        Node curr = root;
        for (int i = 0; i < s.length(); i++) {
            int x = s.charAt(i) - 'a';
            if (curr.arr[x] == null) {
                curr.arr[x] = new Node();
            }
            curr = curr.arr[x];
        }
        curr.has = true;
    }
}
```

